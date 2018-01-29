# fullcalendar-korean

`한국의 모 접근성 인증 기관`에서 구조가 이상하여(... 왜죠...?) 인증을 허가할 수 없다 해서 이전 버전을 베이스로 만든 [fullcalendar](https://github.com/fullcalendar/fullcalendar) 입니다.

2.1.0 버전부터 eventLimit 기능이 생기면서 구조가 대폭 수정되었습니다.
그래서 이전 구조로 돌아가고자 2.0.3 버전으로 다운그레이드를 진행하였지만
다운그레이드시 eventLimit 기능이 되지않아 2.0.3 버전([원본](https://github.com/cdnjs/cdnjs/blob/master/ajax/libs/fullcalendar/2.0.3/fullcalendar.js))을 베이스로 다시 제작하였습니다.

![Imgur](https://i.imgur.com/Jv52wwl.png)
![Imgur](https://i.imgur.com/hNSqKBe.png)

아래 코드는 제가 처리한 설정입니다.
현재 버전의 fullcalendar와 달리 height처리를 통해 사라질 부분을 정합니다. (eventLimit, eventHeight 옵션값 사용)

```
<style>
    .fc-day-content {height: 130px;}
    @media (max-width: 992px){
        .fc-day-content {height: 90px !important;}
    }
    @media (max-width: 767px){
        .fc-day-content {height: 80px !important;}
    }
    @media (max-width: 640px){
        .fc-day-content {height: 45px !important;}
    }
    @media (max-width: 500px){
        .fc-day-content {height: 35px !important;}
    }
    .fc-event-container > .fc-event-more {display: none;}
</style>

<script>
    $('#calendar').fullCalendar({
        header: {
            left: null,
            center: 'prev, title, next',
            right: null
        },
        titleFormat : {
            month : "YYYY년 MM월"
        },
        monthNames : ["1월","2월","3월","4월","5월","6월","7월","8월","9월","10월","11월","12월"],
        monthNamesShort : ["1월","2월","3월","4월","5월","6월","7월","8월","9월","10월","11월","12월"],
        dayNames : ["일요일","월요일","화요일","수요일","목요일","금요일","토요일"],
        dayNamesShort : ["일","월","화","수","목","금","토"],
        navLinks: false, // can click day/week names to navigate views
        editable: false,
        eventLimit: true, // allow "more" link when too many events
        eventHeight: 50, // eventHeight를 지정하여 한 cell에 몇개씩 들어갈지 계산하게 하였습니다.
        events: events,
        eventRender: function(event, element) {
            // TODO
        },
        eventAfterAllRender: function () {
            // TODO

            $('.fc-event-container > .fc-event-more').remove();
            $('.fc-popover').each(function (index, item) {
                var dropdownContainer = $(item);
                var eventCount = dropdownContainer.find('li').length;
                dropdownContainer.find('.more_btn').text('+' + eventCount + '개');
            });

            $('.fc-day-header').attr('scope', 'col');
        }
    });
</script>
```

잘 만든 코드는 결코 아니지만 혹시나 저와 같은 상황에 처해 막막하신 분들이 있으실까 싶어 참고하시라고 올립니다. 

## 문의내용 및 결과
fullcalendar의 내용과 셀의 연관 관계를 알 수 없다는 이유가 주된 이유였습니다.  
테이블 안에 또 다수의 테이블이 들어가 있는 구조라는 것과 캡션이 없다는 것도 언급되었습니다.

### 문의 내용
    안녕하세요.

    XXX 사이트가 접근성 인증을 받았는데 

    XXX의 YYY 페이지에 들어가보시면 fullcalendar 라는 라이브러리를 사용하고 있는데요.

    해당 라이브러리를 사용해도 접근성 인증을 받는데 문제가 따로 없는건가요?
    테이블 안에 또 다수의 테이블이 들어가 있는 구조여서요. 캡션도 따로 없고요.
    확인 부탁드립니다.

    감사합니다.

### 답변1
    1. 실제 인증 심사는 전수가 아닌 샘플링하여 심사가 진행됩니다.
    해당 페이지가 인증 심사 대상에 포함이 되지 않았을 수도 있다는 뜻 입니다.

    2. 제가 간략하게 검수해 본 결과 해당 페이지의 달력 솔루션에는 문제가 없는 것으로 판단됩니다.
    레이아웃 용으로 사용된 <TD>만으로 구성된 표에서는 CAPTION을 제공하면 안됩니다.
    레이아웃을 위한 표를 많이 사용한 경우 스크린 리더 사용자의 사용성이 떨어질 수는 있으나 접근성에 문제가 되지는 않습니다.

### 답변2 (문제의 기관)
(유선으로 진행되었습니다. 텍스트로 남긴건 아니였기 때문에 정확한 녹취는 아닙니다. 의미만 반영하였습니다.)

    A. 해당 라이브러리를 사용한 페이지는 접근성에 문제가 되는게 맞습니다.
    Q. 접근성에는 문제가 없고 사용성만 떨어지는건 아닌가요? 
    A. 셀과 내용과의 연관관계를 알 수 없기때문에 접근성 검사에서 통과되지 않아야 맞습니다. 랜덤으로 페이지 뽑아서 검사가 진행되는데 이 페이지는 검사하지 않은것 같네요. 
    Q. 그러면 안걸리면 된다는걸로 받아들이면 될까요?  
    A. 아니요 그렇지는 않고요  
    Q. 그러면 지금처럼 누락사항이 나오면 경고를 하나요?  
    A. 아니요 그렇지는 않고요, 인증이 발급 된 후 6개월 후 다시 검사를 진행합니다.  

### 결론
접근성 인증은 국가에서 맡겨서 하는 인증 제도인걸로 알고있는데, 기관마다 답이 다르니 답답하네요.  
구체적인 기준이 있었으면 좋겠습니다.

그리고 누락사항이 나왔는데 6개월 후에 검사라니...  
형식적인 접근성 검사는 사라지면 좋겠습니다.  

## 변경로그

### 18.01.26
다 만들었더니 왜 반응형 안되냐고 해서 다시 뜯어 고쳤습니다.(쥬륵...)

### 18.01.29
이전달 다음달 버튼이 tab에 안잡힌다고 수정하였습니다...
