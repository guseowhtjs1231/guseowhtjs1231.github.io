---
title: TIL-장고 튜토리얼 3번째(#3 Django tutorial)
date: "2020-05-07T17:40:32.169Z"
template: "post"
draft: false
slug: "til-django-tutorial-3"
category: "Django"
tags:
  - "Django"
  - "Python"
  - "Wecode"
description: "쟝고를 알기엔 내가 아직 너무 부족하다."
socialImage: "/media/05072020blog.png"
---

![05072020blog.png](/media/05072020blog.png)

https://docs.djangoproject.com/ko/3.0/intro/tutorial03/

 

####쟝고 우리의 장고 

####웹페이지를 쉽게 만들 수 있게 해주는 장고~

####계속 배워본다.


##뷰 추가하기

polls/views.py 를 건든다.

옆의 함수들을 원래 있는 def index 밑에 추가 해준다.

*def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)


def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)*

그리고

polls/urls.py

로 가서 밑의 함수들을 연결시켜준다.

 

*from django.urls import path

from . import views

urlpatterns = [


    ex: /polls/
    path('', views.index, name='index'),
    # ex: /polls/5/
    path('/', views.detail, name='detail'),
    # ex: /polls/5/results/
    path('/results/', views.results, name='results'),
    # ex: /polls/5/vote/
    path('/vote/', views.vote, name='vote'),
]*

 

그니까 저기 urls에서는

views에서 지정한 결과가 나오게 하는 화면? 내가 만들어 놓은 글들에 연결 시켜주는 path함수를 넣어 우리가 url에 치면 연결 시켜준다.

밑에 결과물들을 보면서 이해하면 빠르다.

 

우리가 /polls/34/를 요청하면 쟝고에서는 mysite.urls파이썬 모듈을 불러오고 mysite/setting.py 속 ROOT_URLCONF 설정에 의해 해당 모듈을 바라보도록 지정 되어 있다. mysite.urls에서 urlpatterns라는 변수를 찾고 순서대로 패턴을 따라감.

 

<int:question_id>/

int:이게 어디 문법인지는(js인지..)모르겠지만 정수의 views.py에 넣은 question_id(인자)를 받는다는 것 같다. 다른 path에 넣는 것도 똑같고.

 

 

 


localhost:8000/polls/34/ 결과물

localhost:8000/polls/34/results/ 결과물



localhost:8000/polls/34/vote 결과물
###뷰가 실제로 뭔가 하도록 만들어 보기!

뷰는 실제로 두가지 중 하나를 하도록 되어 있다.

 + 요청된 페이지의 내용이 담긴 HttpResponse 객체를 반환

 + Http404 같은 예외를 발생

 

*하드코딩 이란?

데이터를 코드 내부에 직접 입력하는 것. 기술적으로는 데이터가 실행 바이너리(exe파일 등)에 합쳐져 있는 상태를 말함.*
프로그래머는 하드코딩을 지양하고 최대한 외부로 데이터를 빼는 노력을 해야함.

 

슬슬 이해와 습득의 고리가 끊기는 게 느껴진다. 그래서 블로그 작성하는 시간도 길어진다...

 

*두가지의 상황이 쟝고에게 필요하다.

1. HttpResponse 객체

2. Http404 같은 예외*

 

##HttpResponse


옆의 내용대로 views.py 에 추가해준다. 밑에 주석에 있는 것처럼 다른  것들은 내버려두고 추가한다. 여기서 문제는 옆에 index함수가 하드코딩이 되어 있다는 것이다. 함수 안에 데이터가 입력되어 있다. 이것을 쟝고의 템플릿(template)를 통해서 파이썬 코드로 부터 디자인을 분리할 수 있다.

 

Polls디렉토리에 template라는 디렉토리를 만든다. 그러면 쟝고는 여기서 템플릿 디렉토리를 찾을 것이다. 

 

 


 

템플릿 디렉토리 안에 polls디렉토리를 또 만들고 index.html에 옆의 코드를 붙여 넣는다.

솔직히 오른쪽 코드 어떤 형식으로 적은지는 모르겠지만 대충 for문이 적혀 있는 것으로 보여진다. 

 

 

그리고 위에 polls/views.py에 적은 것을 템플릿 기능을 사용할 수 있게 고쳐쓴다. 


 

latest_question_list 는 그대로인 것을 볼 수 있다. 

그리고 output이 없어지고 template이 생겼다.  .join 으로 넣지 않고 django.template에서 import한 loader를 사용한다. 

loader.get_template('경로')

context도 새로 생겼다. 이거는 return 에 template.render에서 받아쓰려고 들어 가는 것 같다.

 

여기서 render()를 알아봐야한다.

 

 

 


###render() - Shortcuts

템플릿에 context를 채워넣어 표현한 결과를 HttpResponse 객체와 함께 돌려주는 구문을 자주 쓰기 때문에 쟝고(Django)에서 쉽게 표현할 수 있도록 단축기능(Shortcuts)을 제공한다. 그게 render다.

방금 적은 index()를 단축기능을 사용하여 적으면 다음과 같이 된다. 

latest_question_list는 변함없고,

context 도 같다. 그대신 loader.get_template를 쓴 template가 사라졌다.

그리고 바로 return을 

render(request, 'polls/index.html',context)로 받는다. 확실히 위에 것보다 간략하고 단순해졌다.

 

모든 뷰에 적용할 거라면,  loader와 HttpResponse를 import하지 않아도 된다. (만약 detail, results, vote에서 stub 메소드를 가지고 있다면, HttpResponse를 유지해야 한다.) 

Stub 메소드:  다른 프로그래밍 기능을 대리하는 코드, 기존 코드를 흉내내거나 아직 개발되지 않은 코드를 임시로 대치하는 역할.

 

*reunder()*함수는 request 객체를 첫번째 인수로 받고, 템플릿 이름을 두번째 인수로 받으며, context 사전형 객체를 세번째 선택적(optional) 인수로 받는다. 인수로 지정된 context로 표현된 템플릿의 HttpResponse 객체가 반환됨.


###404 에러 일으키기
다시 한번 옆의 그림처럼 detail 을 고쳐본다. 요청된 질문이 없는 질문일 경우 http404예외(excetion)를 발생 시키는 것이다.  일단 옆에 함수를 쳤다면 polls/detail.html을 만들어야된다. {{question}}이라고 임시로 적어놓고 저장시킨다.  그리고 서버를 가동해서 들어가보니 실제로 저렇게 떴다. (밑에 그림 참고)

 

###A shortcut: get_object_or_404()


저 위에서 render와 같은 종류의 친구이다. 시간을 절약시켜주기 위한 좋은 기능이라는 것 같다. 방금 전에 polls/views.py에 적은 길고 긴 함수가 django.shortcuts의 get_object_or_404함수로 인해 매우 간단해 진다. 그림을 참고 하면 알겠지만 다지우고 question이 get_object_or_404함수로 이루어져있고 2가지의 인자만 받는다. 

 

*get_object_or_404* 함수는 django의 모델을 첫번째 인자로 받는다. 그리고 키워드 인자를 몇개 받아 첫번째 인자로 받아진 django 모델의 manager.py의  get()으로 넘겨진다. 객체가 존재 하지 않을때 http404에러가 뜨게 된다.

 

비슷한게 또 있는데

get_list_or_404함수가 있다. 차이점은 get()으로 넘기지 않고 filter()로 넘긴다는 것이다. 객체가 아니고 리스트가 비어있을 때 Http404에러를 뜨게 한다.


템플릿 시스템 사용하기

아까 대충 쓰고 저장했던 detail.html을 왼쪽과 같이 적고 저장한다. 

대충 html보면

h1문단 크기로 질문이 들어가고 

ul - unorderd list로 질문의 선택지들이 줄줄이 나오는 것 같다.

 


오른쪽처럼 텅빈 흰 화면 왼쪽위 구석에 나왔다.

localhost:8000/polls/에 들어가서 질문을 클릭하면 저게 나왔다.

 

그러니 결론은  복잡하게 하드코딩해가며 할 수도 있지만 쟝고의 기능들을 이용해서 편하게 가자 라는 것 같다. 그리고 파이썬 기초문법을 외운 것처럼 장고의 함수들도 외워야 될 것 같다.

 

예전에 튜토리얼 part1할 때 , API가지고 놀기라는 챕터에서

$python manage.py shell

을 쳐서 이것 저것 쳤던 것들인데 그것들이 그대로 저장되어서 화면에 출력되었다.

 

##Removing hardcoded URLs in templates
templates/polls/index.html에 적었던,


하드코딩된 이 것을 template 기능을 활용하여 하드코딩을 없앨 수 있다(?)잘 모르겠지만 일단 의존성을 없앨 수 있다고 한다.


오른쪽 처럼 고칠 수 있다. 왜냐하면 polls.url안에 PATH를 기억하면 된다.


만약 우리가 url을 바꾸고 싶다면 polls.url을 고치면 된다.

 

 

마지막으로 

이름 공간(Namespace)



왼쪽: 변경전 오른쪽 : 네임스페이싱을 한 것
개체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 개체만을 가리키게 된다. 그러니까 쉽게 말하면 똑같은 이름들 헷갈리지 않게 앞에 뭐 붙이자 이런 것. 

그러니까 

강남구 역삼동 봉은사로44길 3호가 나의 주소다라고 하면

강남구 역삼동 이 이름공간 식별자 이다. - namespace identifier

봉은사로 44길 3호가 로컬 이름이다. - local name

 

전국적으로 '봉은사로 44길' 많다고 했을 때 강남구 역삼동을 붙이면 아~ 강남구 역삼동의 봉은사로 구나라고 되는 것처럼 이해하면 될 것 같다. 

네이밍 시스템 (Naming system)

name = <namespace identifier> separator <local name>

 

그러면 위에 코드를 봤을 때

polls가 이름공간 식별자 (namespace identifier)

: 가 구분자 (separator)
detail이 (local name)
 이다. 

 

이런 것을 하는 이유는 여러 개발자나 회사끼리 협업을 할 때에 코드끼리 충돌을 방지하는 것이다. 혼자 개발한다면 크게 필요 없을 거다.

 




<figure>
	<blockquote>
		<p>잠을 자면 꿈을 꾸지만 잠을 이기면 꿈을 이룬다.</p>
		<footer>
			<cite>— Pro Insomnia</cite>
		</footer>
	</blockquote>
</figure>


> 언젠가 낚시배를 사서 주말에 호수에서 낚시를 할 것이다. 오랜 꿈이다.
>
> — Youngbin Ha







