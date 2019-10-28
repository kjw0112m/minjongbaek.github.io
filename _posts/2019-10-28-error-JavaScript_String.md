---
title: "JavaScript에서 문자열 매개변수 입력 오류"
categories: error
date: 2019-08-21 19:37:00
lastmod: 2019-08-22 10:40:00
---

자바스크립트 함수 중 매개변수를 문자열로 처리하고자 할 때 제대로 처리가 안 되는 경우가 있었다.

문자열의 값을 제대로 읽지 못하는 경우였다. 그럴 때는 함수 호출 시 해당 변수에 '\\(역슬래시)'' 처리를 해주면 된다.

 

버튼의 onclick의 속성으로 del() 함수를 호출하는 과정에서 생긴 오류이다.

### <실행 오류 코드>

```javascript
    $(document).ready(function() {
            $.ajax({
                url: 'http://localhost:8080/mm/rest-api/members',
                type: 'GET',
                success: function(data) {
                    //alert(JSON.stringify(data))
                    var html = '';
                    for (var i = 0; i < data.length; i++) {
                        uId = data[i].uId;
                        html += '<div class="card">\n';
                        html += 'idx : ' + data[i].idx + '<br>\n';
                        html += 'uId : ' + data[i].uId + '<br>\n';
                        html += 'uName : ' + data[i].uName + '<br>\n';
                        html += 'uPhoto : ' + data[i].uPhoto + '<br>\n';
                        html += '<button onclick="del(' + data[i].uId + ')">삭..제..하..기..</button><br>\n';
                        html += '</div>\n';
                    }
                    $('#memberlist').html(html);
                }
            });
        });
        function del(uId) {
            alert(uId);
            if (confirm('삭제하시겠습니까?')) {
                $.ajax({
                    url: 'http://localhost:8080/mm/rest-api/members/' + uId,
                    type: 'DELETE',
                    success: function(data) {
                        if (data == 'success') {
                            alert('삭제되었습니다.')
                            list();
                        }
                    }
                });
            }
        }
```



### <수정한 코드>

```javascript
  $(document).ready(function() {
            $.ajax({
                url: 'http://localhost:8080/mm/rest-api/members',
                type: 'GET',
                success: function(data) {
                    //alert(JSON.stringify(data))
                    var html = '';
                    for (var i = 0; i < data.length; i++) {
                        uId = data[i].uId;
                        html += '<div class="card">\n';
                        html += 'idx : ' + data[i].idx + '<br>\n';
                        html += 'uId : ' + data[i].uId + '<br>\n';
                        html += 'uName : ' + data[i].uName + '<br>\n';
                        html += 'uPhoto : ' + data[i].uPhoto + '<br>\n';
                        html += '<button onclick="del(' +'\'' + data[i].uId +'\'' + ')">삭..제..하..기..</button><br>\n';
                        html += '</div>\n';
                    }
                    $('#memberlist').html(html);
                }
            });
        });
        function del(uId) {
            alert(uId);
            if (confirm('삭제하시겠습니까?')) {
                $.ajax({
                    url: 'http://localhost:8080/mm/rest-api/members/' + uId,
                    type: 'DELETE',
                    success: function(data) {
                        if (data == 'success') {
                            alert('삭제되었습니다.')
                            list();
                        }
                    }
                });
            }
        }

```
