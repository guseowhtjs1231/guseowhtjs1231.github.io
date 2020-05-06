---
title: Session- 웹은 으뜨케 작동하는가?(How does the web work?)
date: "2020-05-06T15:00:32.169Z"
template: "post"
draft: false
slug: "How-does-the-web-work"
category: "Session"
description: "코딩을 알기 전에 알아야 할 건 알아야 하지 않겠는가.."
socialImage: "/media/image-3.jpg"
---
일단 호스팅 개념부터 짚고 간다.



##호스팅(Hosting, Web hosting service)
: 인터넷에 띄운다는 것은 홈페이지의 구성파일들이(html, css, js) 인터넷에 "항상" 연결되고, "절대"꺼지지 않는 호스트 컴퓨터(웹 서버)에 저장되어 있다가 사용자의 요청이 오면 언제든 응답.

예) AWS ec2/S3, cafe24 호스팅 센터 등

*A web hosting service is a type of Internet hosting service that allows individuals and organizations to make their website accessible via the World Wide Web. Web hosts are comanies that provide space on a server owned or leased for use by clients, as well as providing Internet connectivity, typically in a data center*

HTML: 웹사이트 구조

CSS : 스타일링

JavaScript: Interaction




간략 설명 그림
##IP
IP 주소는 internet 으로 통신하는 각 device(컴퓨터, 통신장비)에 부여된 고유한 값.

스마트폰이나 노트북부터 대규모 소매 웹 사이트의 콘텐츠를 서비스하는 서버에 이르기까지 인터넷상의 모든 컴퓨터는 숫자를 사용하여 서로를 찾고 통신하며, 이러한 숫자를 IP주소라고 한다. 

An Internet Protocol address (IP address) is a numerical label assigned to each device connected to a computer network that uses the Internet Protocol for communication.



##Domain (Domain name)
문자(string)으로 된 고유 주소. 수많은 IP 주소를 사람이 외워서 접속할 수 없기 때문에 기억하기 쉽다.

ex)www.naver.com , www.google.com 

A domain name is an identification string that defines a realm of administrative autonomy, authority or control within the Internet.


도메인 구조
##DNS (Domain Name System)


 DNS는 사람이 읽을 수 있는 도메인 이름을 머신이 읽을 수 있는 IP 주소로 변환.

DNS는 이름과 숫자 간의 매핑을 관리하여 마치 전화번호부와 같은 기능을 한다. DNS서버는 이름에 대한 요청을 IP주소로 변환하여 최종 사용자가 도메인 이름을 웹 브라우저에 입력할 때 해당 사용자를 어떤 서버를 연결할 것인지를 제어. 이 요청을 쿼리라고 부른다.

예) Amazon Route 53, Cafe24 도메인관리, 가비아 네임서버 관리



*DNS 서버란 도메인과 서버를 연결해주는 중간 서버로, 도메인 이름을 인터넷상의 주소(IP 주소)로 변환시켜 원하는 컴퓨터를 찾아갈 수 있도록 함.




대략 이런 느낌임.




마지막으로



##배포(Deploy)


그동한 개발하던 것을 세상(인터넷상)에 공개하고 모든 사람들이 접근해서 볼 수 있게 하는 것을 의미.








<figure>
	<blockquote>
		<footer>
		<p>
			내 인생에 보증이란 없다.<br>
			 No co-sign in my life
		</p>	
		<cite>— Youngbin John Ha.</cite>
		</footer>
	</blockquote>
</figure>




