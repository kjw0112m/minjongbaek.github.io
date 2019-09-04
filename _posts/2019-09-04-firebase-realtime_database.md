---
title: "firebase 설정"
categories: firebase
date: 2019-09-04 10:50:00
---

팀 프로젝트를 진행하는데, 채팅 기능을 구현해야했다.

자바에서 스레드와, 소켓통신을 이용해 구현할 수 있었지만 웹 채팅은 낯설어 여러 자료를 검색하고, Firbase를 활용해야겠다고 생각했다.



본 포스팅은 Firebase의 기초 설정에 관해서 다뤄보려한다.



> Firebase 홈페이지
>
> https://firebase.google.com/



![1](/_posts/images/2019-09-04-firebase-realtime_database/1.PNG)



firebase의 메인 페이지의 우측 상단 '**콘솔로 이동**' 버튼을 클릭한다.

그 후, '**프로젝트 만들기**' 버튼을 눌러 이름과 애널리틱스 설정을 하고 프로젝트를 생성한다.



Firebase는 호스팅 기능을 지원한다.

Firebase 호스팅은 웹 앱, 정적 / 동적 콘텐츠, 마이크로서비스를 위한 빠르고 안전한 호스팅을 제공한다.

자세한 내용은 https://firebase.google.com/docs/hosting?hl=ko#key_capabilities 문서를 참고하자.



아무튼, Firebase의 여타 서비스들은 Node.js가 필수는 아니지만, Hosting의 경우는 Node.js 설치를 요구하고있다. 따라서 Node.js를 설치해야한다.



> Node.js 홈페이지
>
> [https://nodejs.org](https://nodejs.org/)



![2](/_posts/images/2019-09-04-firebase-realtime_database/2.PNG)



왼쪽의 안정된(?) 버전과 오른쪽의 최신 버전이 있다.

Node.js를 설치 했다면, OS에 맞는 명령프롬포트나 터미널 등을 실행하고 아래의 명령어를 입력한다.

```
npm install -g firebase-tools
```



![3](/_posts/images/2019-09-04-firebase-realtime_database/3.PNG)



위 화면과 같이 정상적으로 설치가 완료되면 아래의 명령어를 입력해 정상적으로 작동하는지 확인합니다.

```
firebase init
```



![4](/_posts/images/2019-09-04-firebase-realtime_database/4.PNG)



+) 만약 'Error: Command requires authentication, please run firebase login'이라는 에러 메시지가 출력된다면  아래 명령어로 로그인을 해주면 해결된다.

```
firebase login
```

