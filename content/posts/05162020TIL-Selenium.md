---
title: TIL - 셀레니움 (Selenium) 기초!!
date: "2020-05-16T22:30:37.121Z"
template: "post"
draft: false
slug: "s-e-l-e-n-i-u-m"
category: "Python"
tags:
  - "Selenium"
  - "Python"
  - "Crawling"
  - "Scraping"
description: "동적웹페이지를 크롤링 할 수 있게 해주는 셀레니움 "
socialImage: ""
---
참고 사이트 : https://www.guru99.com/introduction-to-selenium.html





### 셀레니움은 무엇일까?



: 셀레니움(Selenium)은 다양한 브라우저와 플랫폼에서 웹 응용프로그램의 유효성을 체크하는 무료 오픈소스인 자동 테스트 프레임 워크이다. Java, C#, Python 등과 같은 여러 프로그래밍 언어를 사용하여 Selenium 테스트 스크립트를 작성할 수 있다.




셀레니움 발전도




### 사전 준비 사항



 - 웹드라이버 설치 (브라우저를 제어하기 위해) : https://sites.google.com/a/chromium.org/chromedriver/downloads (크롬유저)

 - 셀레니움(Selenium) 설치 :

' pip install selenium '  or ' conda install -c conda-forge selenium '





### Selenium 시작

python py파일 만들지 않고 그냥 바로 python 쳐서 실행해 봤다.

일단 구글웹드라이버가 실행이 되지 않아서 애먹었다. 보안 문제 인 듯한데 알 수 없는 개발자라고 맥에서 열기를 막아서 혹시나 하는 마음에 이리저리 찾아봤다.(이상한 프로그램이고, 다운받은 홈페이지가 이상한 곳인지) 



크게 이상이 없어 보여 확인없이 열기를 하여서 새로운 창을 얻었다.

일단 

```python ```

from selenium import webdriver

path = '드라이버 파일 위치' User/downloads/chromedriver'  # 확장명은 빼고 입력함.

driver = webdriver.Chrome(path)

```

하니 이렇게 결과가 나왔다.








구글 페이지로 가고 싶다면 

```python

driver.get("https://www.google.com")

#webdriver가 google 페이지에 접속하도록 명령함.

```


구글 창




```python

driver.close()

#webdriver를 종료하여 창을 끈다.

```

매우 단순한 것들을 해보았다. 더 단순한 것들을 해본다.



```python

assert "Google" in driver.title    #내가 구글에 제대로 접속했는지 타이틀 체크

elem = driver.find_element_by_name('q')      # name='q'를 찾아준다.

elem.clear()                              #디폴트값을 지워주고

elem.send_keys("Selenium")    #검색어를 입력한다.

elem.submit()                           #검색을 실행한다.

assert "No results found." not in driver.page_source    #검색이 제대로 되었는지 확인한다.

driver.close()                             #브라우저를 종료한다.

```


구글 검색창을 검사했을 때 가운데 위에서 살짝 오른쪽에 보면 name="q"를 볼 수 있다.
find_elemeny_by_tag_name('h1')

find_element_by_css_selector('#css > div.selector')

find_elemeny_by_class_name('some_class_name')

find_element_by_xpath('/html/body/some/xpath')

find_element_by_id('HTML_id')

이렇게 by_name 외에 단일 element에 접근하는 메소드가 있다.



여러 elements에 접근하는 메소드는 위의 메소드에서 elements로 바꿔주면 된다.



만약 맨위의 assert 구문에 AssertionError가 뜨면 그 error가 생겼을 때의 문제도 고려하여 프로그램을 만들면 된다.




elem.send_key('selenium') 했을 때 화면




번외로 인스타그램 회원 가입하기.

```python

driver.get('http:www.instagram.com/")

element = driver.find_element_by_name("username")

element.clear()

element.send_keys("ThisisID",Keys.TAB)

element = driver.find_element_by_name("password")

element.send_keys("ThisisPASSWORD",Keys.RETURN)

driver.close()



```


input창이 username인 것을 검사를 통해 쉽게 알 수 있다.



위의 코드로 실제로 로그인을 시도한 화면. 잘 적용 되었다.
여기에서 Keys가 나오는데 이거는 



```python

from selenium.webdriver.common.keys import Keys

```

를 넣어주어야 가능하다. 저거를 인풋한 다음 어떤 행동을 취할지 RETURN키를 누를 것인지 TAB인지 ENTER인지 해주는 것이다. 사실 TAB을 해봐야 다시 password의 id나 name을 찾아야 되서 굳이 필요가 없었지만 RETURN키는 바로 로그인을 할 수 있게 해주어서 유용했다. 이로써 셀레니움 기초를 알아봤는데 이걸 활용하여 스타벅스 홈페이지 음료부분을 크롤링 해 볼 예정이다.



