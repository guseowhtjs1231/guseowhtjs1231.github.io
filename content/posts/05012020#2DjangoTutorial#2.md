---
title: "TIL-쟝고 튜토리얼 탐험기02(Explore Django tutorial02)-장고 설계 철학,  Polls.app 만들기"
date: "2020-05-01T15:30:03.284Z"
template: "post"
draft: false
slug: "exploring-the-django-tutorial"
category: "Typography"
tags:
  - "Open source"
  - "Gatsby"
  - "Typography"
description: "German inventor Johannes Gutenberg developed a method of movable type and used it to create one of the western world’s first major printed books, the “Forty–Two–Line” Bible."
socialImage: "/media/gutenberg.jpg"
---
쟝고 접한지 두번째 날이다. 화이팅



##먼저 장고의 설계 철학을 먼저 짚고 가자.





###일반

느슨한 결합 - 느슨한 결합, 탄탄한 응집 : 프레임워크의 각 계층은 정말로 서로 필요하기 전까지는 서로 "알지 못해야" 한다.

적은 코드 - 틀에 박힌 코드 배제 최소한의 코드 활용 , 파이썬의 동적 기능을 최대로 활용

신속한 개발 - 빠른 웹개발 추구

반복하지 말 것 - 중복성은 나쁜 것 , 정규화는 좋은 것 : 최소한의 것들로 최대한의 것을 만든다.

명시적인 것이 묵시적인 것보다 낫다.

일관성



###모델

명시적인 것이 묵시적인 것보다 낫다.

모든 관련 도메인 로직을 포함하라 - 모델을 이해하는 데 요구되는 모든 정보가 모델 내에 있어야 한다.



###데이터베이스 API

SQL 효율성

간결하고 강력한 구문

필요할 경우 SQL 문을 직접 장석하기 쉬워야 함.



###URL 설계

느슨한 결합

무한한 유연성

모범 사례를 장려

명확한 URL



###템플릿 시스템

표현과 로직을 분리

중복을 배제

HTML과의 분리

XML을 템플릿 언어로 사용하지 말 것

디자이너가 코딩 능력이 있는 것으로 가정

공백에 특별한 의미를 부여하지 말 것

프로그래밍 언어를 발명하지 말 것

안전과 보안

확장성



###뷰

단순성

요청 객체의 사용

느슨한 결합

GET 과 POST를 구분



###캐시 프레임워크

적은 코드 

일관성

확장성





##Database setup

mysite/settings.py를 열고 쟝고 세팅을 해볼 수 있다.

기본적으로 SQLite를 데이터베이스로 쓰는데 확장가능한 데이터베이스(PostgreSQL같은)것을 쓰고 싶다면 바꿀 수 있다. 
DATABASES 'default'에서 ENGIN을 바꾸고 NAME도 바꾸면 된다. 원하는 걸로.

예로,

django.db.backends.sqlite3

django.db.backends.postgresql

django.db.backends.mysql

django.db.backends.oracle

등등

name - 나의 데이터베이스 이름. 절대경로로 할때 취급 될 이름. 기본값은 

os.path.join(BASE_DIR, 'db.sqlite3') 이다.



SQLite안쓰고 다른 데이터베이스를 쓰려면 일단 처음부터 고치고 가야하는데

CREATE DATABASE database_name; 치고 하면 된다. 나는 기본값으로 쓸 것이기 때문에 패스한다.



mysite/settings.py 에서 TIME_ZONE을 내가 지금 있는 타임존으로 설정해야한다.



mysite/settings.py에서 INSTALLED_APPS의 기본값 의미.

django.contrib.admin – The admin site. You’ll use it shortly.
django.contrib.auth – An authentication system.
django.contrib.contenttypes – A framework for content types.
django.contrib.sessions – A session framework.
django.contrib.messages – A messaging framework.
django.contrib.staticfiles – A framework for managing static files.
최소 하나의 데이터베이스 테이블을 응용프로그램들이 쓸 것이기 때문에 테이블을 작성해야한다. 그러기 전에 쳐야할 명령어

$ python manage.py migrate

이주 시키는 명령같은데.

migrate 명령어는 setting.py안에 INSTALLED_APPS 세팅을 확인하고 데이터베이스 세팅되어 있는 것에 따라 필요한 데이터베이스 테이블을 만든다.  


python manage.py migrate 실행 결과
각 이주들이 어플라이 되었다고 나온다.



The migrate command will only run migrations for apps in INSTALLED_APPS.

: migrate 명령어는 오직 INSTALLED_APPS의 앱들을 이주하기 위해 사용된다.



두가지의 모델 (질문 과 선택)을 만들어야 되는데 질문은 공식 날짜를 가지고 있고 선택은 두가지의 필드를 가지고 있다. 하나는 글자로된 선택지이고 하나는 투표집계이다. 각 선택은 질문과 연관되어 있다.



이러한 개념은 파이썬으로 표현되는데  polls/models.py을 편집해보자.



polls/models.py

class Question(models.Model):
        question_text = models.CharField(max_length=200)
        pub_date = models.DateTimeField('date published')

class Choice(models.Model):
        question = models.ForeignKey(Question, on_delete=models.CASCADE)
        choice_text = models.Charfield(max_length=200)
        votes = models.IntergerField(default=0)



이러한 각 모델들은 클래스로 인해 표현되는데 django.db.models.Model 의 subclasses이다. 각 모델은 클래스변수의 숫자를 가지는데 각 모델의 데이터베이스 필드를 나타낸다.



데이터베이스의 각 필드는 클래스의 인스턴스로 표현된다. 

CharField 는 문자(Character)필드를, DateTimeField는 날짜와 시간(datetime)필드를 표현함. 이것은 각 필드가 어떤 자료형을 가질 수 있는지를 Django에게 말한다.



각자의 Field 인스턴스의 이름(question_text or pub_date)은 기계가 읽기 좋은 형식(machine-friendly format)의 데이터베이스 필드 이름이다. 이 필드명은 파이썬 코드에서 사용할 수 있으며 데이터베이스는 컬럼명으로 사용할 것이다.



Field클래스의 생성자에 선택적인 첫번째 위치 인수를 전다랗여 사람이 읽기 좋은(human-readable)이름을 지정할 수도 있다.

기계가 읽기 좋은 형태의 이름이라도 사람이 읽기에는 충분하다.



몇몇 Field클래스들은 필수 인수가 필요하다. 예를 들어 Charfield의 경우 max_length를 입력해주어야 한다.  그리고 Field는 다양한 선택적 인수들을 가질 수 있다. 여기 예제에서는 default로 하여금 votes의 기본값을 0으로 선택하였다. 



그리고 ForeignKey를 사용한 관계설정이 있는데, 각각의 Choice가 하나의 Question에 관계된다는 것을 Django에 알려줌. Django는 다-대-일(many-to-one), 다대다(many-to-many), 일대일(One-to-one)과 같은 모든 일반 데이터베이스의 관계를 지원함.



##모델의 활성화

모델에 대한 이 소량의 코드(아까적은 파이썬)가 , Django에게 상당한 양의 정보를 전달함.

Django가 그 정보를 받아 하는 일

 - 이 앱을 위한 데이터베이스 스키마 생성(Create table문)

 - Question과 Choice 객체에 접근하기 위한 Python 데이터베이스 접근 API를 생성



그러나 지금 가장 먼저 해야할 일은 프로젝트에게 Polls앱이 설치되어 있다는 것을 알려야 함.



장고의 앱들은 분리되어 있어서 꼈다 뺐다를 할 수 있다. 



앱을 현재의 프로젝트에 포함시키기 위해서는 mysite/settings.py 파일 속에 installed_app에 

'polls.apps.PollsConfig'를 추가 해야 한다.

이렇게 하면 Django가 polls앱이 포함 된 것을 알게 된다. 



$python manage.py makemigrations polls
makemigrations를 실행시킴으로써, 우리가 모델을 새로만든 사실과 이 사실을 migration에 저장시키고 싶다는 것을 django에게 알린 것이다. 

마이그레이션(Migration)은  모델 및 데이터베이스의 스키마에 대한 변경사항을 저장하는 방법이다. 원한다면 새로운 모델 마이그레이션을 읽을 수 있다.



migrate : 우리를 위해 migration들을 실행시켜주고 자동으로 데이터베이스 스키마를 관리해주는 명령어

sqlmigrate : migration이름을 인수로 받아 실행하는 SQL 문장을 보여줌.



$ python manage.py sqlmigrate polls 0001


python manage.py sqlmigrate polls 0001의 결과화면
지금 껏 알아본 모델 변경을 만드는데 세단계 지침은 다음과 같다.

(  models.py에서) 모델을 변경한다.
Python manage.py makemigrations 를 통해 이 변경사항에 대한 마이그레이션을 만든다.
python manage.py migrate명령을 통해 변경사항을 데이터베이스에 적용한다.
API 가지고 놀기

$python manage.py shell

DJANGO_SETTINGS_MODULE환경변수 때문에 python이라고 치지 않고 위의 명령대로 실행했다. 이렇게 입장을 하고 이것저것 쳐보면 Django에서 동작하는 모든 명령을 대화식 Python 쉘에서 그대로 시험 해 볼 수 있다.



>>>from polls.models import Choice, Question

>>>Question.objects.all()

QuerySet []



>>> from django.utils import timezone

>>> q = Question(question_text="What's new?", pub_date=timezone.now())



>>>q.save()

>>>q.id

1



>>>q.question_text

"What's new?"

>>>q.pub_date

datetime.datetime(2020, 5, 1, 2, 46, 51, 523795, tzinfo=)
>>> q.question_text = "What's up?"
>>> q.save()
>>> Question.objects.all()

<QuerySet [Question: Question object(1)]>
>>> exit()



<QuerySet [Question: Question object(1)]>은 이 객체를 표현하는 데 별로 도움이 되지 않는다. (polls/models.py파일의)Question모델을 수정하여, __str__()메소드를 Question과 Choice에 추가하면 도움이 된다.

polls/models.py

from django.db import models



class Question(models.Model):

      def __str__(self):

            return self.question_text

class Choice(models.Model):

      def __str__(self):

            return self.choice_text



우리의 모델에 __str__()메소드를 추가하는 것은 객체의 표현을 대화식 프롬프트에서 편하게 보려는 이유 말고도, Django가 자동으로 생성하는 관리 사이트에서도 객체의 표현이 사용되기 때문이다.



또 모델에 커스텀 메소드를 하나 추가해본다.

polls/models.py

import datetime



from django.db import models

from django.utils import timezone



class Question(models.Model):

      def was_published_recently(self):

            return self.pub_date >= timezone.now() - datetime.timedelta(days=1)



import datetime은 Python 의 표준 모듈인 datetime모듈을 참조하기 위해

from django.utils import timezone은 Django의 시간대 관련 유틸리티인 django.utils.timezone을 참조하기 위해 추가 한 것이다.



다시 

$python manage.py shell

을 하고 API에서 놀아보자




API에서 이것저것 실험


##관리자 생성하기



$python manage.py createsuperuser

이렇게 하면

username과 Email address와 Password를 2번 묻는다. 다 입력을 하면 슈퍼유저 계정이 생긴다.



##개발 서버 시작



$python manage.py runserver

한 다음 

http://127.0.0.1:8000/admin/

 으로 접근하면 유저네임과 비밀번호를 입력하라고 한다.



##관리자 사이트에 들어가기

앞에서 생성한 슈퍼유저계정으로 로그인을 하면 Django관리 인덱스 페이지가 보인다.



편집 가능한 그룹과 사용자와 같은 몇 종류의 컨텐츠를 볼 수 있다. 이것들은 django.contrib.auth 모듈에서 제공되는데 , Django에서 제공되는 인증 프레임 워크이다.



관리사이트에서 poll app을 변경가능하도록 만들기

polls/admin.py

from django.contrib import admin

from .models import Question

admin.site.register(Question)



이제 polls Questions가 생겼다. 들어가보면 수정도 가능하고 히스토리도 볼 수 있다.!!


이로써 튜토리얼 2장이 끝났다. 




<figure>
	<blockquote>
		<p>
		 	특별한 것이 넘쳐나는 곳에선 특별하지 않는 것이 특별한 것이다.
		</p>
		<footer>
			<cite>Youngbin Ha</cite>
		</footer>
	</blockquote>
</figure>
