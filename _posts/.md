---
title: "Node.js socket을 이용한 웹 채팅 기능 구현"
categories: nodejs
date: 2019-09-09 18:20:00
---

본 포스팅은 express를 설치하고, socket.io에 대한 라이브러리를 설치한 후의 과정입니다.



### **코드**

------

```javascript
// express
var app = require("express")();
// http, app
var http = require("http").Server(app); // 1
// socket.io
var io = require("socket.io")(http);

// get 요청
app.get("/", function(req, res) {
  res.send("<h1>Hello</h1>");
});

// user 구분을 위한 변수
function getRandomCode(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

// socket 연결 성공시 콘솔 출력
io.on("connection", function(socket) { // 2
  // user 구분을 위한 변수
  var usercode = getRandomCode(1, 9999);
  var name = "user" + usercode;

  io.to(socket.id).emit("user_name", name); // 3
  console.log(
    "socket.id : " +
      socket.id +
      ", username : " +
      name +
      "이(가) 접속했습니다..!"
  );

  io.sockets.emit("alarm", `${name}이(가) 입장..!`); // 4

  // socket 연결 종료시 콘솔 출력
  socket.on("disconnect", function() {
    console.log(
      "socket.id : " +
        socket.id +
        ", username : " +
        name +
        "이(가) 접속종료했습니다..!"
    );
    io.sockets.emit("alarm", `${name}이(가) 퇴장..!`);
  });

  // 콘솔 출력
  socket.on("send_msg", function(msg) {
    console.log(name + ":" + msg);

    // 메시지는 자신을 제외한 다른 유저들에게 보낸다.
    socket.broadcast.emit("receive_msg", `[${name}] : ${msg}`); // 5

    // 내가 보낸 메시지를 확인하기 위한 처리
    socket.emit("send_msg", `[${name}] : ${msg}`); // 6
  });
});

// 82번 포트를 서버로 사용
http.listen(82, function() {
  // 콘솔창에 출력
  console.log("listening on *:82");
});

```



1. socket.io를 사용하는 경우는 app을  http에 연결한 후, 이 http를 다시 socket.io에 연결 시킨다. (socket.io가 express를 직접 받지 못하기 때문..!)
2. **io.on("이벤트 명", 함수( ))**는 서버에 전달된 이벤트를 인식하여 함수를 실행시키는 이벤트 리스너이다. 사용자가 웹 사이트에 접속하면 'connection' event가 발생한다.
3. **socket.emit("이벤트 명", 데이터)**는 현재 연결되어 있는 클라이언트 소켓에 "이벤트 명"으로 이벤트를 전달한다.
4. **io.sockets.emit("이벤트 명", 데이터)**는 나를 포함한 모든 클라이언트들에게 이벤트를 전달한다.
5. **socket.broadcast.emit("이벤트 명" 데이터)**는 나를 제외한 다른 클라이언트들에게 이벤트를 전달한다.