---
title: "WebRTC #1 - 개요"
categories: nodejs
date: 2019-11-15 21:30:00
classes: wide
---

 WebRTC를 사용

WebRTC를 이용해 화상채팅 기능을 구현하면서 배우고 이해했던 것에 대해 공유하려고 한다.



## **WebRTC란?**

> WebRTC는 브라우저 및 모바일 애플리케이션에서 RTC (Real-Time Communications) 를 편리하게 할 수 있는 api를 만들고자 하는 목적으로 시작된 프로젝트입니다. 
>
> 무료 공개 프로젝트이지만 Google, Mozilla 및 Opera가 지원하는 프로젝트입니다.
>
> WebRTC로 할 수 있는 일은 많지만, 간단한 p2p 영상 / 음성 연결, p2p 데이터 교환 등에도 많이 쓰입니다.
>
> [WebRTC 공식 홈페이지](https://webrtc.org/)



## **시그널링**

WebRTC로 P2P 통신을 하기 위해서는 서로 다른 두 Peer가 네트워크 정보를 교환하는 `시그널링`과정이 필요하다. 

시그널링은 WebRTC에서 제안하는 표준이 없기 때문에 개발자에 따라 구현 형태도 다르고, 정답도 없다.



### 네트워크 정보 교환하기

네트워크 정보를 교환하기 위해서 ICE 프레임워크를 사용한다. ICE는 각 Peer가 서로 최적의 네트워크 경로를 찾을 수 있도록 STUN 과 TURN 서버를 활용하여 여러 연결가능한 후보(Candidate)를 검출하고 이들 중 하나를 선택하여 Peer 간 연결을 수행한다.



### NAT

STUN과 TURN 서버를 설명하기 위해서는 NAT이 무엇인지 조금(?) 알아야한다.

NAT은 네트워크를 외부망과 분리하고 공인망과 내부망의 IP주소와 포트 번호를 매핑해주는 역할을 한다.

<img src="/assets/images/posts/2019-11-15-nodejs-WebRTC1/nat.PNG" alt="nat" style="zoom: 80%;" />

위 그림을 보면, 192.168.100.3:3885에 대해서 NAT는 외부에 145.12.131.7:6282로 매핑해서 전달을 한다. 외부에서 보면 단순히 145.12.131.7:6282로 접속하면 되는 것 처럼 보이지만 NAT의 종류에 따라서 달라진다.

NAT 종류는 다른분들의 포스팅을 참고하도록 하자...



### STUN

STUN 서버는 다음 2가지의 임무를 수행한다.

 	1. 어떤 peer가 NAT / Firewall 뒤에 있는지를 판단하게 해준다.
 	2. 어떤 peer에 대한 Public IP Address를 결정하고 NAT / Firewall의 유형에 대해서 알려준다.

STUN은 p2p 연결을 위한 정보를 제공해주기만 할 뿐, p2p 연결이 불가능할 경우에 연결을 위해 STUN 서버가 해줄 수 있는 것은 없다.

<img src="/assets/images/posts/2019-11-15-nodejs-WebRTC1/stun.PNG" alt="stun" style="zoom: 80%;" />

즉, STUN 서버는 접속 가능한 IP 주소와 포트번호를 찾는 역할을 한다.



### TURN

TURN은 peer간 직접 통신이 실패할 경우 종단점들 사이에서 데이터 릴레이를 수행한다. 

<img src="/assets/images/posts/2019-11-15-nodejs-WebRTC1/turn.PNG" alt="turn" style="zoom: 80%;" />

### ICE

ICE는 클라이언트의 모든 통신 가능한 주소를 식별한다. 그리고 이를 위해 다음 3가지를 활용한다.

- Local Address : 클라이언트의 사설 주소, 랜과 무선랜 등 다수 인터페이스가 있으면 모든 주소가 후보가 된다.
- Server Reflexive Address : NAT 장비가 매핑한 클라이언트의 공인망 주소로 STUN에 의해 판단한다.
- Relayed Address : TURN 서버가 패킷 릴레이를 위해 할당하는 주소이다.

아래의 두 그림 처럼 각 접속 경로들로 상호 패킷들이 오가면서 전송속도, 안정성 등을 판단해 미디어 연결을 한다.

<img src="/assets/images/posts/2019-11-15-nodejs-WebRTC1/ice1.PNG" alt="ice1" style="zoom: 80%;" />

<img src="/assets/images/posts/2019-11-15-nodejs-WebRTC1/ice2.PNG" alt="ice2" style="zoom: 80%;" />



## 참고 자료

> Getting Started with WebRTC : https://www.html5rocks.com/ko/tutorials/webrtc/basics/ 
>
> WebRTC Basic : https://cryingnavi.github.io/WebRTC-Basic/
>
> STUN과 TURN에 대하여 : https://alnova2.tistory.com/1110