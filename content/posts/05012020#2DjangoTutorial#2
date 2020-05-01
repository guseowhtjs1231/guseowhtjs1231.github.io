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



German inventor Johannes Gutenberg developed a method of movable type and used it to create one of the western world’s first major printed books, the “Forty–Two–Line” Bible.

**Johannes Gensfleisch zur Laden zum Gutenberg** (c. 1398 – 1468) was a German blacksmith, goldsmith, printer, and publisher who introduced printing to Europe. His invention of mechanical movable type printing started the Printing Revolution and is widely regarded as the most important event of the modern period. It played a key role in the development of the Renaissance, Reformation, the Age of Enlightenment, and the Scientific revolution and laid the material basis for the modern knowledge-based economy and the spread of learning to the masses.

<figure class="float-right" style="width: 240px">
	<img src="/media/gutenberg.jpg" alt="Gutenberg">
	<figcaption>Johannes Gutenberg</figcaption>
</figure>

With his invention of the printing press, Gutenberg was the first European to use movable type printing, in around 1439. Among his many contributions to printing are: the invention of a process for mass-producing movable type; the use of oil-based ink; and the use of a wooden printing press similar to the agricultural screw presses of the period. His truly epochal invention was the combination of these elements into a practical system that allowed the mass production of printed books and was economically viable for printers and readers alike. Gutenberg's method for making type is traditionally considered to have included a type metal alloy and a hand mould for casting type. The alloy was a mixture of lead, tin, and antimony that melted at a relatively low temperature for faster and more economical casting, cast well, and created a durable type.

In Renaissance Europe, the arrival of mechanical movable type printing introduced the era of mass communication which permanently altered the structure of society. The relatively unrestricted circulation of information — including revolutionary ideas — transcended borders, captured the masses in the Reformation and threatened the power of political and religious authorities; the sharp increase in literacy broke the monopoly of the literate elite on education and learning and bolstered the emerging middle class. Across Europe, the increasing cultural self-awareness of its people led to the rise of proto-nationalism, accelerated by the flowering of the European vernacular languages to the detriment of Latin's status as lingua franca. In the 19th century, the replacement of the hand-operated Gutenberg-style press by steam-powered rotary presses allowed printing on an industrial scale, while Western-style printing was adopted all over the world, becoming practically the sole medium for modern bulk printing.

The use of movable type was a marked improvement on the handwritten manuscript, which was the existing method of book production in Europe, and upon woodblock printing, and revolutionized European book-making. Gutenberg's printing technology spread rapidly throughout Europe and later the world.

His major work, the Gutenberg Bible (also known as the 42-line Bible), has been acclaimed for its high aesthetic and technical quality.

## Printing Press

Around 1439, Gutenberg was involved in a financial misadventure making polished metal mirrors (which were believed to capture holy light from religious relics) for sale to pilgrims to Aachen: in 1439 the city was planning to exhibit its collection of relics from Emperor Charlemagne but the event was delayed by one year due to a severe flood and the capital already spent could not be repaid. When the question of satisfying the investors came up, Gutenberg is said to have promised to share a “secret”. It has been widely speculated that this secret may have been the idea of printing with movable type. Also around 1439–1440, the Dutch Laurens Janszoon Coster came up with the idea of printing. Legend has it that the idea came to him “like a ray of light”.

<figure class="float-left" style="width: 240px">
	<img src="/media/printing-press.jpg" alt="Early Printing Press">
	<figcaption>Early wooden printing press as depicted in 1568.</figcaption>
</figure>

Until at least 1444 he lived in Strasbourg, most likely in the St. Arbogast parish. It was in Strasbourg in 1440 that Gutenberg is said to have perfected and unveiled the secret of printing based on his research, mysteriously entitled Kunst und Aventur (art and enterprise). It is not clear what work he was engaged in, or whether some early trials with printing from movable type may have been conducted there. After this, there is a gap of four years in the record. In 1448, he was back in Mainz, where he took out a loan from his brother-in-law Arnold Gelthus, quite possibly for a printing press or related paraphernalia. By this date, Gutenberg may have been familiar with intaglio printing; it is claimed that he had worked on copper engravings with an artist known as the Master of Playing Cards.

By 1450, the press was in operation, and a German poem had been printed, possibly the first item to be printed there. Gutenberg was able to convince the wealthy moneylender Johann Fust for a loan of 800 guilders. Peter Schöffer, who became Fust’s son-in-law, also joined the enterprise. Schöffer had worked as a scribe in Paris and is believed to have designed some of the first typefaces.

<figure>
	<blockquote>
		<p>All that has been written to me about that marvelous man seen at Frankfurt is true. I have not seen complete Bibles but only a number of quires of various books of the Bible. The script was very neat and legible, not at all difficult to follow—your grace would be able to read it without effort, and indeed without glasses.</p>
		<footer>
			<cite>—Future pope Pius II in a letter to Cardinal Carvajal, March 1455</cite>
		</footer>
	</blockquote>
</figure>

Gutenberg's workshop was set up at Hof Humbrecht, a property belonging to a distant relative. It is not clear when Gutenberg conceived the Bible project, but for this he borrowed another 800 guilders from Fust, and work commenced in 1452. At the same time, the press was also printing other, more lucrative texts (possibly Latin grammars). There is also some speculation that there may have been two presses, one for the pedestrian texts, and one for the Bible. One of the profit-making enterprises of the new press was the printing of thousands of indulgences for the church, documented from 1454–55.

In 1455 Gutenberg completed his 42-line Bible, known as the Gutenberg Bible. About 180 copies were printed, most on paper and some on vellum.

## Court Case

Some time in 1456, there was a dispute between Gutenberg and Fust, and Fust demanded his money back, accusing Gutenberg of misusing the funds. Meanwhile the expenses of the Bible project had proliferated, and Gutenberg's debt now exceeded 20,000 guilders. Fust sued at the archbishop's court. A November 1455 legal document records that there was a partnership for a "project of the books," the funds for which Gutenberg had used for other purposes, according to Fust. The court decided in favor of Fust, giving him control over the Bible printing workshop and half of all printed Bibles.

Thus Gutenberg was effectively bankrupt, but it appears he retained (or re-started) a small printing shop, and participated in the printing of a Bible in the town of Bamberg around 1459, for which he seems at least to have supplied the type. But since his printed books never carry his name or a date, it is difficult to be certain, and there is consequently a considerable scholarly debate on this subject. It is also possible that the large Catholicon dictionary, 300 copies of 754 pages, printed in Mainz in 1460, may have been executed in his workshop.

Meanwhile, the Fust–Schöffer shop was the first in Europe to bring out a book with the printer's name and date, the Mainz Psalter of August 1457, and while proudly proclaiming the mechanical process by which it had been produced, it made no mention of Gutenberg.

## Later Life

In 1462, during a conflict between two archbishops, Mainz was sacked by archbishop Adolph von Nassau, and Gutenberg was exiled. An old man by now, he moved to Eltville where he may have initiated and supervised a new printing press belonging to the brothers Bechtermünze.

In January 1465, Gutenberg's achievements were recognized and he was given the title Hofmann (gentleman of the court) by von Nassau. This honor included a stipend, an annual court outfit, as well as 2,180 litres of grain and 2,000 litres of wine tax-free. It is believed he may have moved back to Mainz around this time, but this is not certain.

***

Gutenberg died in 1468 and was buried in the Franciscan church at Mainz, his contributions largely unknown. This church and the cemetery were later destroyed, and Gutenberg's grave is now lost.

In 1504, he was mentioned as the inventor of typography in a book by Professor Ivo Wittig. It was not until 1567 that the first portrait of Gutenberg, almost certainly an imaginary reconstruction, appeared in Heinrich Pantaleon's biography of famous Germans.

## Printing Method With Movable Type

Gutenberg's early printing process, and what tests he may have made with movable type, are not known in great detail. His later Bibles were printed in such a way as to have required large quantities of type, some estimates suggesting as many as 100,000 individual sorts. Setting each page would take, perhaps, half a day, and considering all the work in loading the press, inking the type, pulling the impressions, hanging up the sheets, distributing the type, etc., it is thought that the Gutenberg–Fust shop might have employed as many as 25 craftsmen.

![Movable metal type, and composing stick, descended from Gutenberg's press. Photo by Willi Heidelbach. Licensed under CC BY 2.5](/media/movable-type.jpg)

*Movable metal type, and composing stick, descended from Gutenberg's press. Photo by Willi Heidelbach. Licensed under CC BY 2.5*

Gutenberg's technique of making movable type remains unclear. In the following decades, punches and copper matrices became standardized in the rapidly disseminating printing presses across Europe. Whether Gutenberg used this sophisticated technique or a somewhat primitive version has been the subject of considerable debate.

In the standard process of making type, a hard metal punch (made by punchcutting, with the letter carved back to front) is hammered into a softer copper bar, creating a matrix. This is then placed into a hand-held mould and a piece of type, or "sort", is cast by filling the mould with molten type-metal; this cools almost at once, and the resulting piece of type can be removed from the mould. The matrix can be reused to create hundreds, or thousands, of identical sorts so that the same character appearing anywhere within the book will appear very uniform, giving rise, over time, to the development of distinct styles of typefaces or fonts. After casting, the sorts are arranged into type-cases, and used to make up pages which are inked and printed, a procedure which can be repeated hundreds, or thousands, of times. The sorts can be reused in any combination, earning the process the name of “movable type”.

The invention of the making of types with punch, matrix and mold has been widely attributed to Gutenberg. However, recent evidence suggests that Gutenberg's process was somewhat different. If he used the punch and matrix approach, all his letters should have been nearly identical, with some variations due to miscasting and inking. However, the type used in Gutenberg's earliest work shows other variations.

<figure>
	<blockquote>
		<p>It is a press, certainly, but a press from which shall flow in inexhaustible streams… Through it, god will spread his word.</p>
		<footer>
			<cite>—Johannes Gutenberg</cite>
		</footer>
	</blockquote>
</figure>

In 2001, the physicist Blaise Agüera y Arcas and Princeton librarian Paul Needham, used digital scans of a Papal bull in the Scheide Library, Princeton, to carefully compare the same letters (types) appearing in different parts of the printed text. The irregularities in Gutenberg's type, particularly in simple characters such as the hyphen, suggested that the variations could not have come from either ink smear or from wear and damage on the pieces of metal on the types themselves. While some identical types are clearly used on other pages, other variations, subjected to detailed image analysis, suggested that they could not have been produced from the same matrix. Transmitted light pictures of the page also appeared to reveal substructures in the type that could not arise from traditional punchcutting techniques. They hypothesized that the method may have involved impressing simple shapes to create alphabets in “cuneiform” style in a matrix made of some soft material, perhaps sand. Casting the type would destroy the mould, and the matrix would need to be recreated to make each additional sort. This could explain the variations in the type, as well as the substructures observed in the printed images.

Thus, they feel that “the decisive factor for the birth of typography”, the use of reusable moulds for casting type, might have been a more progressive process than was previously thought. They suggest that the additional step of using the punch to create a mould that could be reused many times was not taken until twenty years later, in the 1470s. Others have not accepted some or all of their suggestions, and have interpreted the evidence in other ways, and the truth of the matter remains very uncertain.

A 1568 history by Hadrianus Junius of Holland claims that the basic idea of the movable type came to Gutenberg from Laurens Janszoon Coster via Fust, who was apprenticed to Coster in the 1430s and may have brought some of his equipment from Haarlem to Mainz. While Coster appears to have experimented with moulds and castable metal type, there is no evidence that he had actually printed anything with this technology. He was an inventor and a goldsmith. However, there is one indirect supporter of the claim that Coster might be the inventor. The author of the Cologne Chronicle of 1499 quotes Ulrich Zell, the first printer of Cologne, that printing was performed in Mainz in 1450, but that some type of printing of lower quality had previously occurred in the Netherlands. However, the chronicle does not mention the name of Coster, while it actually credits Gutenberg as the "first inventor of printing" in the very same passage (fol. 312). The first securely dated book by Dutch printers is from 1471, and the Coster connection is today regarded as a mere legend.

The 19th century printer and typefounder Fournier Le Jeune suggested that Gutenberg might not have been using type cast with a reusable matrix, but possibly wooden types that were carved individually. A similar suggestion was made by Nash in 2004. This remains possible, albeit entirely unproven.

It has also been questioned whether Gutenberg used movable types at all. In 2004, Italian professor Bruno Fabbiani claimed that examination of the 42-line Bible revealed an overlapping of letters, suggesting that Gutenberg did not in fact use movable type (individual cast characters) but rather used whole plates made from a system somewhat like a modern typewriter, whereby the letters were stamped successively into the plate and then printed. However, most specialists regard the occasional overlapping of type as caused by paper movement over pieces of type of slightly unequal height.
