---
layout: post
title:  " HTTP vs Socket "
categories: Network
author: goodGid
---
* content
{:toc}


# HTTP 통신 vs Socket 통신

* 통신을 하는데 있어서 다음과 같이 크게 두 가지로 나눌 수 있다.

1. HTTP 통신
2. Socekt 통신


* HTTP와 Socket의 가장 큰 차이점은 <b>접속(Connection)</b>을 유지하는지의 여부이다.


---

## HTTP 통신

HTTP 통신은 웹브라우저에 정보를 표시하는 것과 같이 클라이언트의 요청이 있을 때 

서버가 해당 페이지에 대한 자료를 전송하고 곧바로 연결을 끊는 방식이다. 

현재 이 글을 보고 있는 상황에 맨 처음 이 페이지를 로드 시에만 서버와 연결이 되고

현재는 서버와 접속이 끊어진 상태이다.

이 상태에서 F5 키를 눌러 새로고침을 하거나 다른 페이지로 이동하면 그때 다시 서버와 연결을 한다.

<br>

이렇게 하는 이유는 단 한가지. 서버의 부하를 줄여서 다른 접속을 원활하게 처리하기 위해서이다. 

만약 F5 키에 연필을 꽂아서 클라이언트가 서버를 계속해서 물고 늘어지면 

서버는 이 클라이언트의 연결을 유지하느라 다른 컴퓨터의 응답이 늦어질 것이다.

이런 방식으로 여러 대의 PC가 서버를 붙잡고 늘어져서 서버가 다른 일을 하지 못하도록 하는 것을 <b>DDOS</b> 공격이라한다.


---

## Socket 통신

Socket 통신은 클라이언트가 서버와 접속이 되면 서버나 클라이언트에서 강제로 접속을 해제할 때까지는 계속해서 접속이 유지된다.

따라서 서버의 능력이 무한대가 아닌 이상 동시에 접속할 수 있는 클라이언트의 수가 제한이 될 수 밖에 없다.

Socket 통신은 실시간으로 정보 교환이 필요하는 채팅이나 온라인 게임, 실시간 동영상 강좌 등에 사용된다.

따라서 이와 같은 경우가 아니라면 서버와의 통신은 HTTP를 사용하는 것이 시스템의 자원을 보다 효과적으로 사용할 수 있다.

