---
title: TIL-쟝고 간단 정리(Django review)
date: "2020-05-11T22:46:37.121Z"
template: "post"
draft: false
slug: "Django-review"
category: "Django"
tags:
  - "Blog"
  - "Django"
description: "간략하게 두편의 영상과 간단한 자료를 이용하여 쟝고 리뷰와 정리 "
socialImage: "/media/image-2.jpg"
---
##Django는



MVC & MTV

 - Model

 - View

 - Control, Template(Django)






어떻게 돌아가는지
그림 출처:https://www.essenceandartifact.com/2012/





##Django의 개념

모델

##Project와 App
Project는 하나의 사이트라고 보면 되고 

App은 하나의 기능들이다. 



내가 naver.com를 만들고 싶다. 그러면 그게 프로젝트가 되는 것이고

각각의 이메일 기능, 뉴스 볼 수 있는 기능, 지식인 기능, 검색 기능 등이 하나의 App 이 되어 하나하나 따로 관리한다.



django-admin startporject tutorial

 - 튜토리얼이라는 프로젝트 생성

/manage.py startapp communit

 - Polls라는 app을 생성



우리가 자주 만지고 쓸 것은 

models.py

views.py

tutorial/settings.py

urls.py

manage.py



###Settings.py
 - DEBUG : 디버그 모드 설정 (배포하기 전엔 꺼야 됨)

 - INSTALLED_APPS : pip로 설치한 앱 또는 본인이 만든 app을 추가

 - MIDDLEWARE_CLASSES : request와 response 사이의 주요 기능 레이어(보안 쪽)

 - TEMPLATES :  django template 관련 설정, 실제 뷰(html, 변수)

 - DATABASES : 데이터베이스 엔진의 연결 설정

 - STATIC_URL : 정적 파일의 URL(css, javascript, image, etc.)



###Manage.py
 - 프로젝트 관리 명령어 모음

 - 주요 명령어

    + startapp - 앱 생성

    + runserver - 서버 실행

    + createsuperuser - 관리자 생성

    + makemigrations app - app의 모델 변경 사항 체크

    + migrate - 변경 사항을 DB에 반영

    + shell - 쉘을 통해 데이터를 확인

    + collectstatic - static 파일을 한 곳에 모음



 - ex) /manage.py runserver 0.0.0.0:8080




###Article APP

name

title

contents

url

email

cdate


app디렉터리 안에 url.py 속 urlpatterns에 내가 넣을 것들 추가해주고

views.py에 가서 

from django.shortcuts import render

def write(request): #request는 사용자가 넣을 것

      return render(request, 'write.html')#template write.html을 쓰겠단 소리.


해주고! 그리고 template 만들러 가면 됨

wirte.html 파일 만들고 기본 뭐 넣고 싶은 대로 html형식 맞춰서 넣으면 됨. 



대충 옆에처럼 저렇게 적으면 연동이 다 된 것임.



지금까지만 한 것으로는 localhost:8080/write 에는 hello django! 만 뜬다.



이제 게시판 기능 모델을 사용하여서 폼을 넣어보자










view.py에

오른쪽처럼 코드를 끼워 넣자




그리고 template에 저 폼을 설정해주는데 



이거는 굳이 필요 없지만 html을 이렇게 해주자. 그러면 오른쪽 그림처럼 홈페이지 화면에 뜰 것이다.



views.py  수정


이렇게 if구문을 넣어준다.  만약 POST면 폼에 넣고 폼에 있는 데이터가 유효하면 데이터베이스에 폼을 세이브하고, 아니면 폼을 놔둔다.

근데 이렇게 하고 그냥 홈페이지 가서 폼에 입력을 하고 저장을 하면 CSRF 뭐라고 하면서 금지된 접근이라고 함.(403이 뜸)



그래서 write.html 가서 


csrf 추가
간단한 문구 , {% csrf_token %}을 추가해준다. 그리고 저장을 누르면 저장이 된다. 근데 저장이 되어도 그 화면은 그대로 떠있을 것이다.

그러면 여기서 list화면을 만들어준다. 화면을 만들어야 하니까

Views.py로 간다.


Views.py에 추가된 list
이렇게 마지막에 넣어주고

template에  위의 그림에 나와있는 list.html을 만들어 줘야 한다.  근데 그전에 리스트를 가져와서 표시해줘야 하기 때문에 표시될 함수를 넣어주자.


list가 나열될 수 있도록 articlelist를 추가했다.
그리고 list.html(template)를 만들어 주자.(이거는 프론트 역할)


저장된 글들이 표시될 리스트의 폼을 html형식으로 만들어줌


unorderd list태그를 이용하여 폼을 만들어주었다.


표시된 화면
이렇게 나온다. 

그런데 여기 리스트에서 저 글을 보고 싶어 누를 수 있게 해주는 view기능을 추가할 것이다.

다시 urls.py 가서 


urlpatterns에 추가 된 view URL
다시 Views에 가서 view를 정의해준다.


뷰에 뷰를 정의해줌
이제 어디갈지 딱 눈치챈 것과 같이 view.html를 만들고


아까와 비슷하게 만들어 준다.

그리고 저장한다.



그리고 홈페이지 가서 view/하고 원하는 숫자를 넣으면 몇 번째 게시글을 보여준다.


뷰기능이 추가되어 게시글을 보는 화면
이제 list.html 가서 링크를 넣어주면 됨.


그러면 이렇게 뙇 게시판이 만들어진다. 



반복 작업하면서 조금 외워진 것처럼 



모델을 만들고 간단하게 데이터베이스에 접근이 가능하고

url 가서 정규표현식으로 urlpatterns 가서 넣고(유저들이 접근 가능하게)

그리고 views에 가서 리퀘스트받는 작업들을 정의해주고

template가 필요하다면 html작업을 하여 연결시켜주면 

브라우저에 뜨는 화면까지 완벽하게 된다. 그렇게 되면 유저들이 쓸 수 있는 홈페이지가 되는 것이다.



장고라는 도구를 이용하여 뚝딱뚝딱 웹서비스를 만드는 작업을 해보았다. 영상을 캡처하면서 한 것이기 때문에 내가 한 것은 아니지만 하나하나 이해하면서 넘겼다. 



출처 :  https://www.youtube.com/channel/UCCZum2rAfq3SjjUFshYDZww





