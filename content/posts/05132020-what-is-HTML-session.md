---
title: Session - HTTP 구조 및 핵심 요소
date: "2020-05-13T14:46:37.121Z"
template: "post"
draft: false
slug: "about-html-session"
category: "Session"
tags:
  - "HTML"
  - "Session"
  - "Wecode"
  - "HTTP"
description: "HTTP에 대하여 알고 가자~ "
socialImage: "/media/image-2.jpg"
---
##HTTP
 - HyperText Transfer Protocol

 - HTTP는 TCP/IP 기반으로 되어있다. 

- 하이퍼텍스트 문서를 교환하기 위해 만들어진 Protocol(통신 규약)

 + 즉 웹상에서 네트워크 서버기리 통신을 할 때 어떠한 형식으로 서로 통신을 하자고 규정해 놓은 "통신 형식" 혹은 "통신 구조"라고 보면 됨

 + 프론트앤드 서버와 클라이언트 간의 통신에 사용된다.

 + 또한 백엔드와 프론트앤드 서버간에의 통신에도 사용된다.



##HTTP 핵심 요소
###HTTP  통신 방식
1. HTTP 기본적으로 요청/응답 (request/response) 구조로 되어있다.

 - 클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response를 보내는 구조.

 - 클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.

2. HTTP는 Stateless이다.

 - Stateless란 말 그대로 state(상태)를 저장하지 않는다는 뜻.

 - 즉, 요청이 오면 그에 응답을 할 뿐, 여러 요청/응답 끼리 연결되어 있지 않다는 뜻이다. 즉 각각의 요청/응답은 독립적인 요청/응답이다.

 - 예를 들어, 클라이언트가 요청으 ㄹ보내고 응답을 받은후, 조금 있다 다시 요청을 보낼 때, 전에 보낸 요청/응답은 독립적인 요청/응답이다.

 - 그래서 만일 여러 요청과 응답의 진행과정이나 데이터가 필요할 때는 쿠키나 세션 등등을 사용하게 된다.



###HTTP Request 구조
HTTP request 메시지는 크게 3부분으로 구성된다.

####1. start line
 - 말 그대로 HTTP request의 첫 라인

 - HTTP request의  start line 또한 3부분으로 구성되어 있음.

 - HTTP Method : 해당 request가 의도한 action을 정의하는 부분.(GET, POST, PUT, DELETE, OPTIONS 등등이 있다.)

 - Request target : 해당 request가 전송되는 목표 URL. ex) /login

 - HTTP version : 말 그대로 사용되는 HTTP 버전, 예) GET /search HTTP/1.1



####2. Headers
 - 해당 request에 대한 추가 정보(additional information)를 담고 있는 부분.

ex) request 메시지 body의 총길이(Content-Length) 등

 - Key:Value 값으로 되어있다.(':'이 사용됨)

ex) key:value , HOST: google.com

 - Headers는 크게 3부분으로 나눠진다.(General headers, request headers, entity headers)

 - 자주 사용되는 header의 정보 : 

HOST : 요청이 전송되는 target의 host url 예) google.com

User-Agent : 요청을 보내는 클라이언트의 대한 정보. 예) 웹브라우저에 대한 정보

Accept : 해당 요청이 받을 수 있는 응답(response) 타입.

Connection : 해당 요청이 끝난 후에 클라이언트와 서버가 계속해서 네트워크 커넥션을 유지할 것인지 끊을건지 지시하는 부분
Content-Type : 해당 요처잉 보내는 메시지 body의 타입. 예를 들어, JSON을 보내면 application/json

Content-Length : 메시지 Body의 길이



####3. Body
 - 해당 request 의 실제 메시지/내용

 - Body가 없는 request도 많다. GET request들은 대부분 body가 없는 경우가 많음.



#####HTTP Response 구조

Response도 request와 마찬가지로 크게 3부분으로 구성되어 있다.

######1. Status line

 - Response의 상태를 간략하게 나타내주는 부분

 - 3부분으로 나눠진다. HTTP 버전, Status code(응답 상태를 나타내는 코드), Status text(응답 상태를 간략하게 설명)

######2. Headers

 - Request의 Headers와 동일하다.

 - Response에서만 사용되는 Header 값들이 있다. 예) User-agent 대신 Server사용

######3. Body

 - Request의  body와 일반적으로 동일하다.

 - Request와 마찬가지로 모든 response가 body가 있지 않다. 데이터를 전송할 필요가 없을 경우  Bdoy가 비어있게 된다.



###자주 쓰이는 HTTP Methods
#####Get
 - 이름 그대로 어떠한 데이터를 서버로 부터 받아올 때 주로 사용한다.

 -  데이터 생성/수정/삭제 없이 받아오기만 할 때 사용된다.

 - 가장 간단하고 많이 사용되는 HTTP Method

 - 언급한대로 주로 데이터를 받아올 때 사용되기 때문에 request에 body를 안 보내는 경우가 많다.

 

#####POST
 - 데이터를 생성/수정/삭제 할 때 주로 사용된다.

 - 데이터를 생성 및 수정 할 때 많이 사용하기 때문에 대부분의 경우 request  body가 포함되서 보내진다.



#####OPTIONS
 - 주로 요청 URI에서 사용할 수 있는 메소드

 - 예를들어, /update URI에서 어떤 method를 요청 가능한가(GET or POST)를 알고 싶으면 먼저 OPTIONS 요청을 사용해서 확인하게 된다.

 

#####PUT
 - POST와 비슷하다. 데이터를 생성 할 때 사용되는 메소드.

 - POST와 겹치기 때문에 PUT을 사용하는 곳도 있고 POST로 통일해서 사용하는 곳도 있는데, 최근엔 POST에 밀려서 잘 사용안함.



#####DELETE
 - 특정 데이터를 서버에서 삭제 요청을 보낼 때 쓰이는 메소드

 - PUT과 마찬가지로 POST에 밀려서 잘 사용안되는 추세.



###자주 쓰이는 HTTP Status Code
######200 OK

 - 가장 자주 보게 되는 status code.

 - 문제 없이 다 잘 실행 되었을 때 보내는 코드.



######301 Moved Permanently

 - 해당 URI가 다른 주소로 바뀌었을 때 보내는 코드.



######400 Bad Request

 - 해당 요청이 잘못된 요청일 때 보내는 코드

 - 주로 요청에 포함된 input값들이 잘못되었을 때 사용되는 코드



######401 Unauthorized

 - 유저가 해당 요청을 진행 하려면 먼저 로그인을 하거나 회원 가입을 하거나 등등이 필요하다는 것을 나타낼 때 쓰임.



######403  Forbidden

 - 유저가 해당 요청에 대한 권한이 없다는 뜻



######404 Not Found

 - 요청된 URI가 존재 하지 않는 다는 뜻.



######500 Internal Server Error

 - 서버에서 에러가 났을 때 사용되는 코드

 - API 개발을 하는 백엔드 개발자들이 싫어하는 코드.





여기까지 미리 나눠준 공부할 거리.



상태가 없다? stateless이기 때문에 불편한 점.



매번 요청때에 로그인 했다는 것을 매 요청마다 포함시켜서 보내야한다. (쿠키와 세션 이용해서)쿠키가 길고 세션이 짦음

로그인을 했다는 사실을 HTML은 인식을 하지 못하기 때문에.

토큰활용하여 왔다갔다함.



