# fullcalendar-korean

한국의 모 접근성 인증 기관에서 접근성에는 전혀 걸리지 않으나 구조가 이상하여(... 왜죠...?) 인증을 허가할 수 없다 해서 이전 버전을 베이스로 만든 [fullcalendar](https://github.com/fullcalendar/fullcalendar) 입니다.

2.1.0 버전부터 eventLimit 기능이 생기면서 구조가 대폭 수정되었습니다.
그래서 이전 구조로 돌아가고자 2.0.3 버전으로 다운그레이드를 진행하였지만
다운그레이드시 eventLimit 기능이 되지않아 2.0.3 버전([원본](https://github.com/cdnjs/cdnjs/blob/master/ajax/libs/fullcalendar/2.0.3/fullcalendar.js))을 베이스로 다시 제작하였습니다.

![Imgur](https://i.imgur.com/Jv52wwl.png)

아래 코드는 제가 처리한 설정입니다.
현재 버전의 fullcalendar와 달리 height처리를 통해 사라질 부분을 정합니다. (eventLimit, eventLimitHeight 옵션값 사용)

```
<style>
    .fc-day-content {height: 130px !important;} /* 셀 높이 고정 */
    .fc-event-container > .fc-event-more {display: none;} /* 사라져야 할 이벤트 보이지 않도록 선처리 */
</style>

<script>
$(function () {
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
        eventLimitHeight: 80,
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
                dropdownContainer.find('.more_btn').text('+' + eventCount);
            });
        }
    });
})
</script>
```

잘 만든 코드는 결코 아니지만 혹시나 저와 같은 상황에 처해 막막하신 분들이 있으실까 싶어 참고하시라고 올립니다. 
