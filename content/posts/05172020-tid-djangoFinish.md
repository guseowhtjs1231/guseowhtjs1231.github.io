---
title: TID-장고 글올리기, 회원가입, 로그인(Comment, Sign-up, Sign-in)기능 만들기
date: "2020-05-17T20:32:37.121Z"
template: "post"
draft: false
slug: "django-start-app-account-comment"
category: "Django"
tags:
  - "Sunday"
  - "TodayIDid"
description: "장고 로그인과 회원가입, 글쓰기 기능 구현 "
socialImage: "/media/image-2.jpg"
---
![capture1.png](/media/capture1.png)

휴 오늘 주옥같은 멘토의 피드백을 도강하고 완성한 나의 기능들... 여러 곳 짜집기를 했지만(무에서 유를 창조하진 못함ㅜㅜ) 그래도 나만의 스타일로 바꾸고 여러 에러들을 잡으려고 노력 많이 했다. 내가 생각한 에러들은 다 막았다. 적당히 성취감을 느끼는 작업이였다. ㅎㅎ

##회원가입 기능

지난 번에 올렸던 기능들을 갈아엎을 건 엎고 했는데 비슷한 것도 많이 있다. 일단 첫번째로 tree를 올리겠다.

두가지의 앱을 만들었다.

-   account
    
-   comment
    

그리고 프로젝트의 이름은 westa 이다.

회원가입 기능은 account라는 앱으로 만들었고 sign-up과 sign-in 두가지 메소드를 만들었다.

일단 첫번째로  
###models.py

```
python

from django.db import models
from django.conf import settings
from django.utils import timezone

class  Account(models.Model):
	name = models.CharField(max_length = 50)
	email = models.CharField(max_length = 100, unique=True)
	password = models.CharField(max_length = 300)
	created_at = models.DateTimeField(auto_now_add = True)
	updated_at = models.DateTimeField(auto_now = True)

	class  Meta:
		db_table = 'account_account'

```

예전부터 여러번 말했는데,

models.py 는 데이터베이스에 넣을 형식들을 정해주는 곳이다.

내가 account라는 앱에서 받을 데이터들은 총 5가지이다. (name, email, password, created\_at, updated\_at)

meta클래스로 db\_table 이름을 지정해준다. 사실 메타 클래스의 indentation을 잘 못 넣어줘서 장고에서 지정한 이름으로 테이블이 만들어졌다. 그게 account\_account이다. 그래서 저렇게 지정한 척 했다^^.

두번째로,

\### Views.py

```
python

import json    #먼저 라이브러리 받아주고


from django.views import View
from django.http import JsonResponse, HttpResponse      #장고 내장모듈

from .models import Account     # 내가 만든 모듈 순서로 넣는다.

class  SignUpView(View):    #회원가입 기능의 시작!!
	def  post(self, request):   # 정보들을 밀어넣을 때에!!어떻게 할 것인지 정의
		try:            		# 일단 Body의 정보들을 json.loads로 불는 것을 data에 정의를 한다.
			data = json.loads(request.body)
			if Account.objects.filter(email =data['email']).exists(): 
				return JsonResponse({"message":"ALREADY_SIGNED_UP_EMAIL"}, status=400)
			Account.objects.create(
				name = data['name'],
				email = data['email'],
				password = data['password'],
                ).save()
			#복잡한 것 같지만 Account.objects에 필터기능을 써서 email이 있을 경우 "ALREADY"메시지를 출력
            #else니까 등록된 이메일이 없을 경우 정상적으로 진행!! 아까 모델스에 정의했던 것들을 데이터로 받음 그리고 세이브
			return HttpResponse(status=200)
		#정상 작동 코드 200을 리턴해줌
        except  KeyError:
			return JsonResponse({"message":"NO_DATA_ENTERED"}, status=405)
				
def  get(self, request):
	user_data = Account.objects.values()
	return JsonResponse({'account':list(user_data)}, status=200)
#사실 이기능은 여기서 크게 필요는 없지만 초반에 실험단계일 때 넣었던 것. get으로 땡겼을 때 가입된 이메일과 이름 비밀번호를 리턴

class  SignInView(View):
	def  post(self, request):
		try:
			data = json.loads(request.body)
			try:
				if Account.objects.filter(email = data['email']).exists():
					user = Account.objects.get(email=data\['email'])
					if user.password == data['password']:
	#이게 좀 복잡한데 이메일이 같을 경우 패스워드를 비교한다. 패스워드가 있으면 정상 200코드 리턴!! 없으면 아래의 순서대로 오류 메시지를 내보냄
    #하지만 비밀번호를 잘못쳤는지 이메일을 잘못쳤는지는 알려주지 않음. 내부적으로는 코드로 분류를 하여 핸들한다.
                        return HttpResponse(status=200)
					
                    return JsonResponse({"message":"WRONG_ID_OR_PASSWORD"},status=401)
				
                return JsonResponse({"message":"WRONG_ID_OR_PASSWORD"},status=400)

			except  KeyError:
				return JsonResponse({"message":"INVALID_KEYS"}, status=400)
		except  KeyError:
			return JsonResponse({"message":"NO_DATA_ENTERED"}, status=405)

```

데이터들을 받는 어떻게 보면 api를 핸들하는 views.py 다.

구구절절한 설명을 덧붙였다.

그 다음으로 건드린 것은...

### ###account/urls.py

```
python
from django.urls import path
from .views import SignUpView, SignInView
#URL을 이걸로 핸들해준다. import를 잘 해줄것
app_name = "account"

urlpatterns = [
	path('/sign-up', SignUpView.as_view()),
	path('/sign-in', SignInView.as_view()),
]

```

### ###westa/urls.py

```
python

from django.urls import path, include

urlpatterns = [
	path('account',include('account.urls')),
]
#account 라는 단어를 끼고 있는 URL이라면 account.urls를 참조한다.
```

이로써 account sign-up과 sign-in기능이 구현되었다. 테스트를 해보면 다음과 같다.

![capture2.png](/media/capture2.png)
![capture3.png](/media/capture3.png)
---

이제

## ##comment app

이다.

위와 같이

models.py부터 건드려 본다.

### ###comment/models.py

```
python

from django.db import models
from django.conf import settings
from django.utils import timezone


class  Comment(models.Model):
	name = models.CharField(max_length = 50)
	contents = models.CharField(max_length = 3000)
	created_at = models.DateTimeField(auto_now_add = True)
	updated_at = models.DateTimeField(auto_now = True)
#데이터양식들을 지정해줌.
	class  Meta:
		db_table = 'comment'
		#데이터베이스의 테이블을 지정해줌 'comment'로.
```

### ###comment/views.py

```
python

import json

from django.views import View
from django.http import JsonResponse, HttpResponse

from .models import Comment
from account.models import Account

#가입된 사람의 이름만 받는 comment기능을 만들고 싶어서 Account를 account.models에서 import했다.
#이름이 있을 경우에만 comment가 되고 아니면 가입부터하라는 에러메시지를 출력한다.
#모든 에러를 잡으려고 노력하다보니.. 필요없는 부분도 많이 있을 것이다.
#get같은경우는 comment가 없을 경우 노 코멘트라고 출력하게 했다. 

class  CommentView(View):
	def  post(self, request):
		try:
			data = json.loads(request.body)
			if Account.objects.filter(name = data['name']).exists():
				Comment.objects.create(
					name = data['name'],
					contents = data['contents'],
                    ).save()
				return HttpResponse(status=200)
			return JsonResponse({"message":"SIGN_UP_FIRST"}, status=400)
	# return JsonResponse({"message":"TRY_AGAIN"}, status=401)

		except  KeyError:
			return JsonResponse({"message":"NO_DATA_ENTERED"}, status=405)
		
        except Account.DoesNotExist:
			return JsonResponse({"message":"SIGN_UP_FIRST"}, status=404)

	def  get(self, request):
		comment_data = Comment.objects.values()
		if  len(comment_data)==0:
			return JsonResponse({'Message':'NO_COMMENTS'}, status=404)
		return JsonResponse({'Comment':list(comment_data)},status=200)
#get할 경우 comment의 리스트를 리턴한다.
```

### ###comment/urls.py

```
python

from django.urls import path
from .views import CommentView

#as you know commentview가 나오게 해준다.

app_name = "comment"
urlpatterns = [
	path('', CommentView.as_view()),
]

```

### ###westa/urls.py

```
python

from django.urls import path, include

urlpatterns = [
	path('account',include('account.urls')),
	path('comment',include('comment.urls')),
]
#account 와  comment 두 app이 사이좋게 나란히 적혀있다.
```

위에서 까먹고 말 안한 것!!

### **###westa/settings.py**

에서 INSTALLED\_APPS를 업데이트 해주는 것

```
python

INSTALLED_APPS = [
#	'django.contrib.admin',
#	'django.contrib.auth',
	'django.contrib.contenttypes',
	'django.contrib.sessions',
	'django.contrib.messages',
	'django.contrib.staticfiles',
	'account',
	'comment'
]
#django장고가 앱을 인식하게 해줌.
```

이로써 comment 기능의 구현이 끝났다. 잘 작동 되는지 확인 할 수 있다.

![capture4.png](/media/capture4.png)

잡을 수 있는 에러들을 더 잡아야 하지만 사질 잘 모르겠다. 없다고 믿고 싶다.



