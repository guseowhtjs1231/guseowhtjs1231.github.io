---
title: "Session-웹서비스에 대해서(about Web Service)"
date: "2020-05-05T17:00:32.169Z"
template: "post"
draft: false
slug: "a-session-what-is-webservice"
category: "Session"
tags:
  - "Wecode"
  - "Wework"
  - "Weare"
  - "youngbin"
description: "나는 꿈꾸고 상상한다. 누구나 원하는 것을 이루면 좋겠다라고"
socialImage: "/media/image-0.jpg"
---
#쟝고 - 2세대형 웹프레임워크  ( 백엔드 프론트엔드)



예전에는 서버개발자냐 아니냐  끝 나머지는 웹퍼블리셔 웹디자이너 뿐이였음





리액트가 나오면서 각자의 영역이 뚜렷해짐.



쟝고에서는

models와 views가 중요함.

+models - 프레임

+views - 로직, 컨트롤러



기본적으로 쟝고에서 이 3가지만 쓰면 됨.

 - models

 - urls.py

 - views - controller





백엔드인 우리에게 중요한 것은.

 - API

 - END Point



쟝고의 가장 강력한 기능

ORM 

Oriented-Relational Mapping





API 설계/ 구현을 위해서 알아야 할 것

 - Database

 - Web, HTTP

 - AWS,cloud환경 (서버)









쿼리스트링 - 서버에게 요청하는 문자열 , 검색 시스템은 get이게 하는 것.







RESTful

HTTP

API
를 만들어야 되는데 이 세가지의 규칙을 알아야 함.

이 세가지의 맞춘 API가 개발되어야됨.



##RESTful
“Representational State Transfer” 의 약자

자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미한다.

##REST구성
 - 자원(Resource) - URI

 - 행위(Verb) - HTTP METHOD

 - 표현(Representations)


##REST의 특징
###1) Uniform (유니폼 인터페이스)

Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일을 말합니다.



###2) Stateless (무상태성)

REST는 무상태성 성격을 갖습니다. 다시 말해 작업을 위한 상태정보를 따로 저장하고 관리하지 않습니다. 세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 됩니다. 때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해집니다.



###3) Cacheable (캐시 가능)

REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능합니다. 따라서 HTTP가 가진 캐싱 기능이 적용 가능합니다. HTTP 프로토콜 표준에서 사용하는 Last-Modified태그나 E-Tag를 이용하면 캐싱 구현이 가능합니다.



###4) Self-descriptiveness (자체 표현 구조)

REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 자체 표현 구조로 되어 있다는 것입니다.



###5) Client - Server 구조

REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로간 의존성이 줄어들게 됩니다.



###6) 계층형 구조

REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 합니다.



###REST API 디자인 가이드

중요한 2가지

####첫 번째, URI는 정보의 자원을 표현해야 한다.(리소스명은 동사보다는 명사를 사용)

####두 번째, 자원에 대한 행위는 HTTP Method(GET, POST,PUT,DELETE)로 표현한다.



##주의할 점

1) 슬래시 구분자(/)는 계층 관계를 나타내는 데 사용

2) URI 마지막 문자로 슬래시(/)를 포함하지 않는다.

3) 하이픈(-)은 URI 가독성을 높이는 데 사용

4) 밑줄(_)은 URI에 사용하지 않는다.

5) URI 경로에는 소문자가 적합하다.

6) 파일 확장자는 URI에 포함시키지 않는다.

 





##HTTP
request 와 response가 서로 주고받음. 

1.request 
start line

method - get, post, head, put , delete,

get은 body에 데이터가없다

post는 body에 데이터가 있다. 이렇게 하기러 한것이다.



response될 타겟팅 목표가 엔드포인트 

쟝고의 urls.py가 엔드포인트를 만듬.



headers



body

json을 주고받을 수 있음.



2.response - 요청에 대한 응답 
status line. 상태라인? http version , 상태코드.

응답코드가 있다.200 정상, 404, 401, 500(알수없는에러) 등등

header에 담기는 것들

cookies
인증 토큰


body

데이터를 바디에 실어 보냄.

데이터타입 xml을 많이 썼으나 지금은 json(데이터타입)을 씀.







외우면 좋을 것 

가상환경만들기

매 프로젝트마다 새로만들어서 하는 것이 좋음.

conda create -n test03 python=3.8

하고

conda activate test03



그리고 

pip install django 하고 가상환경

django-admin

으로 설치확인

명령어 확인



outer test03은 쓸모없고

inner test03이 중요한 것.



manage.py가 있는 곳이 물리적 최상위 공간



web server service 할 때 

wsgi.py

web service gateway interface가 필요함.

파이썬을 웹서비스하려면 필요함



asgi는 파이썬 3.0부터 나옴 비동기 통신 서버를 위해서 나옴. 사례가 많지 않음.

assingcorss gateway interface?



세팅스를 봄.

debug는 개발때에는 true이고 배포하고나서는 false



installed app

어드민과 어스는 비활성화 주석처리

+ admin  - 관리자 기능

+ auth  - 로그인 기능

middleware

csrf - 페이지 위변조하는지 체킹하는 보안기능

auth - 로그인 기능

비활성화





root_urlconf = ‘test03.urls’  기본 기준이 되는 url



템플릿은 페이지만들때 쓰는거라서 넘어감 



wsgi_application 은 웹서비스할 때 필요한 것.



databases는 데이터베이스 내가 뭐쓸지 뭘로 관리할지.



auth_password_validators

비밀번호 체크하는 검사하는 기준들 . 우리는 사용안함. 할꺼면 따로 만들고.



Use_tz - false하면 내 컴퓨터시간으로 됨.





###vim 단축키

+dd 는 그라인 삭제

+yy는 그라인 복사

+p는 붙여넣기

+u는 undo

+cc는 오려두기



###쟝고 서버가동 방법

python manage.py runserver

런서버는 데이터베이스없으며 ㄴ안됨







내컴퓨터를 가르키는 localhost or  127.0.0.1

DNS

도메인네임을 ip로 바꾸는 시스템.

<figure>
	<blockquote>
		<p>
			세상에 내 주머니에 돈꼽아주려는 사람은 없다.
		</p>
		<footer>
			<cite>- Misuk Lee.</cite>
		</footer>
	</blockquote>
</figure>




```

