---
title: TIL-장고 튜토리얼5..(Django tutorial 5)
date: "2020-05-11T17:26:37.121Z"
template: "post"
draft: false
slug: "django-tutorial-5"
category: "TIL"
tags:
  - "Django"
  - "NotEasy"
  - "Tutorial"
  - "AlmostDone"
description: "곧 튜토리얼의 끝이 다가온다... "
socialImage: "/media/image-2.jpg"
---
###자동화된 테스트 소개
자동화된 테스트를 소개시켜주나 보다.

나의 코드를 검사해주는 자동화 테스트.  잘 되나 잘 돌아가나 내가 원하는 것이 잘 나오는가? 라는 것을 해주는 장치를 장고에서 쉽게 만들게 도와준다. 여러 모듈이 있고 여러 함수를 쓰면 편하게 테스팅을 해준다.



어느 시점까지는 작동이 잘되고 어느 지점에서 오류가 나는지 거슬러 올라갈 때에 이런 것들이 필요하지 않을까?

시간이 지나고 숙련도와 전문성이 높아진다면 지금보다 많고 복잡한 상호 작용속에 놓이게 될텐데 그 때에 이런 것들이 필요할 것 같다.



###테스트를 만들어야 하는 이유
 - 문제를 식별하는 것이 아니라 예방한다.

 - 코드를 더 매력적으로 만든다.

 - 팀이 함께 일하는 것을 돕는다.

 - 시간을 절약할 수 있다.



테스트를 작성해볼텐데

일단

###버그 식별하기

오른쪽의 그림처럼 shell 을 이용해 의도적으로 발생하게 만든 버그를 찾아본다. 일단 future_question자체가 미래를 지정해놓았다. 미래를 지정해놓고 was_published_recently()라는 예전에 저장해놓은 함수를 썼 때에 False가 나와야 되는데 True가 나오는 버그를 발견할 수 있다. 버그라는 놓친 부분이나 실수한 부분들이 대부분이지 않을까? 컴퓨터는 실수 안하니까 . 우리가 이렇게 수작업으로 찾은 이 버그를 자동화된 테스트로 만들 수 있다. 어떻게? 이렇게! 밑에 그림을 보면된다.
















어플리케이션 테스트는 일반적으로 어플리케이션의 tests.py파일에 있다. 쟝고의 테스트 시스템은 test로 시작하는 파일에서 테스트를 자동으로 찾는다.



오른쪽과 같은 코드를 tests파일에 집어 넣었다.

미래로 지정되어있는 질문은 당연히 최근 질문이 아니기때문에 False가 나와야 한다. 



그래서 



django.test.TestCase 하위클래스(QuestionModelTests)를 만들었다. 미래의 pub_date를 가진 Question 인스턴스를 생성하는 메소드다.



그리고















python manage.py test polls
 를 실행하면 


오른쪽같은 화면이 뜬다. 선 실습 후 블로그작성중이라 나의 화면은 어디있는지 모르겠지만 저런게 떳었다. 우리가 작성한 tests.py가 어떻게 작동이 되었나 보면



 - manage.py test polls는 polls 애플리케이션에서 테스트를 찾음

 - django.test.TestCase클래스의 서브 클래스를 찾음

 - 테스트 목적으로 특별한 데이터베이스 만듦

 - 테스트 메소드 - 이름이 test로 시작하는 것들을 찾음

 - test_was_published_recently_with_future_question에서  pub_date필드가 30일 미래인 Question인스턴스를 생성함

 - ...assertIs() 메소드를 사용하여, 우리가 False가 반환되기를 원함에도 불구하고 was_published_recently()가 True를 반환한다는 것을 발견함.



오른쪽 화면을 잘 보면 1개의 테스트를 0.001초만에 실행했고 테스트 결과들을 알려준다. 


우리는 이미 문제가 무엇인지 알고 있다. 그래서 고치면 된다.

Polls/models.py가서 오른쪽 그림처럼 함수를 수정해주자.

그리고 나서 테스트를 다시 실행하면 




오른쪽과 같이 기분좋은 화면이 뜬다. 사실 기분 그렇게 좋지 않을 수도 있는데 오타와 indentation문제 때문에 몇번이나 수정을 했다. 그래서 성공적인 테스트 결과화면을 보기가 힘들었다.











### View 테스트
####뷰에 대한 테스트

우리가 사용할 수 있는 도구 리스트

장고 테스트 클라이언트 : 장고는 뷰 레벨에서 코드와 상호 작용하는 사용자를 시뮬레이트하기위해 테스트 클라이언트 클래스 client를 제공함. test.py를 이용할 때와 다른 2가지의 사전행동을 해야함.


#####1. shell에서 테스트 환경 구성

오른쪽 그림 처럼 모듈 들고와서 셋업을 해준다. 

setup_test_environment()는

response.context와 같은 response의 추가적인 속성을 사용할 수 있게 한다. 이 메소드는 테스트 데이터베이스를 셋업하지 않음. 그렇기 때문에 테스트는 현재 사용중인 데이터베이스 위에서 돌게 되며 결과는 데이터베이스에 이미 만들어져 있는 질문들에 따라서 조금 달라질 수 있음. 

#####2. text client class를 import

from django.test.utils import 를 해줘야 한다.


####뷰 개선시키기

아직 우리가 게시는 하지 않았지만 곧 올릴 설문조사는 Pub_date가 있는 것으로 될 것이다.

오른쪽의 그림대로 views.py에 적혀있을 것이다. 여기서 

*from django.utils import timezone*

을 위에 추가해주고

*def get_queryset(self):*

*      return Question.objects.filter(pub_date__lte = timezone.now()).order_by('-pub_date')]:5]*

로 수정해준다. 

*Question.objects.filter(pub_date__lte = timezone.now())*는 *timezone.now*보다 pub_date가 작거나 같은 Question을 포함하는 queryset을 반환한다.



####새로운 뷰 테스트
runserver 를 해서 admin으로 들어가니 실제로 다 잘되고 있다. 다시한번 쉘에 기초를 둔 테스트를 작성해보자



polls/tests.py에 *from django.urls import reverse*를 추가한다.

그리고 몇가지의 함수와 클래스를 tests.py에 넣는다.



class QuestionIndexViewTests의 메소드들
넘 길어서 한장에 캡쳐가 안되었다. create_question만 따로 define하고 나머지는 class안에 있다. 

여기를 살펴보면

 - 밖에서 define한 create_question은 테스트 과정 중 설문을 생성하는 부분에서 반복 사용된다. 

 - test_no_questions는 질문을 생성하지는 않지만 "사용가능한 투표가 없다"라는 메시지를 띄우고 latest_question_list가 비어있는지 확인한다.

 - test_past_question에서 우리는 질문을 생성하고 그 질문이 리스트에 나타나는지 확인한다.

 - test_future_question에서 우리는 미래의 pub_date로 질문을 만든다. 데이터베이스는 각 테스트 메소드마다 재설정되므로 첫 번째 질문은 더 이상 존재하지 않으므로 다시 인덱스에 질문이 없어야 한다.



이런 것들이 예상하는 결과가 출력되는지 확인하기 위한 것이다.

assertContains()와 assertQuerysetEqual()이 사용되었다. 두가지의 인자는 이런식이다. 첫 번째 인자에 response를 넣고 두번째 인자에 출력될 것을 적는다.














####DetailView테스트하기


views에 detailView를 적어준다. 주석에서 볼 수 있듯이

Excludes any questions that aren't published yet.

아직 배포되지않은 질문들은 제외시킨다. 날짜가 되지 않은 것들은 숨기는 작업을 여기에도 해줘야 한다.








그리고 시간이 지난 pub_date값을 가지고 있는 설문은 표시되고, 미래의 pub_date는 표시되지 않게 몇가지 검사를 추가한다. 오른쪽 처럼 tests.py에 추가한다.



이로써 대충 실습은 끝났다. 테스트는 많이 할 수록 좋다고 한다.

이건 뭐 실제 사용되는 코드보다 테스트 코드들이 더 많아질 수도 있다고 생각했는데 여기서도 그런 것에 대해 걱정하지 마라고 한다. 테스트의 최신화나 신경쓰고 테스트 습관을 들이는게 중요하다고 한다. 



######경험에 근거한 좋은 테스트 방법

 - 각 모델이나 뷰에 대한 별도의 TestClass

 - 테스트하려는 각 조건 집합에 대해 분리된 테스트 방법

 - 기능을 설명하는 테스트 메소드 이름





프론트와 달리 실제 동작들을 바로바로 확인하기가 어려운데 그런 것들을 해주는 장치도 있다. LiveServerTestCase가 장고에도 있다. 



복잡한 어플리케이션을 사용하는 경우 연속적으로 통합하기 위해 모든 커밋마다 자동으로 테스트를 실행하여 품질 제어가 적어도 부분적으로 자동화되도록 할 수도 있다.



