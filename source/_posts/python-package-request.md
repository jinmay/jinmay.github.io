---
title: requests
tags:
  - package
  - python
  - request
categories:
  - archive
  - python
date: 2018-01-04 17:51:06
---


# HTTP for humans
python에서 기본적으로 제공하는 라이브러리로 urllib가 있다. 하지만 보다 더 간결하고 쉽게 사용할 수 있는 서드파티 패키지인 **requests**를 사용해보려고 한다. JavaScript 처리가 필요한 경우에 selenium을 사용하곤 했는데 requests를 통해서도 어느정도는 js를 처리할 수 있다고 한다.

> selenium은 리소스를 많이 차지하므로 requests를 먼저 사용해보고 안된다면 selenium을 사용해보자.

## 사용법

### GET 요청

~~~python
# 단순한 GET 요청
import requests
response = requests.get("http://news.naver.com/")

# 상태코드
response.status_code

# 헤더
response.headers

# html
response.text
~~~

사람이 이해하기 쉬운 HTTP 핸들링 라이브러리를 표방하는 만큼 사용되는 메소드가 매우 직관적임을 알 수 있다.



### GET 요청 시 커스텀헤더 지정 - 옵션명 : **headers**

~~~python
custom_header = {
    'User-Agent': ('Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 '
						'(KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'),
  	'Referer': 'http://news.naver.com/main/home.nhn',
}

response = requests.get('http://news.naver.com/main/main.nhn', headers=custom_header)
~~~

기존 header에 덮어쓰기를 실행한다

requests 라이브러리의 기본 User-Agent 값은 : python-requests 인데 서버에 따라 User-Agent 값으로 응답을 거부하는 경우가 있기때문에 주의하여야 한다.



### GET 인자 지정 - 옵션명 : **params**

~~~python
# Dict 이용 - 동일key의 인자를 다수 지정 불가
get_params = {'first': 'a', 'second': 'b', 'third': 'c'}
response = requests.get(url, params=get_params)

# list or tuple 이용 - 동일key의 인자를 다수 지정 가능
get_params =(('k1', 'v1'), ('k1', 'v2'), ('k2', 'v2'))
response = requests.get(url, params=get_params)
~~~



### 응답헤더

~~~python
# header
response.header

# header의 Content-Type 확인
response.header['Content-Type']

# encoding 확인
# Content-Type의 charset 값으로 획득한다
response.encoding
~~~



### 응답 body - 두 가지 타입이 존재한다

1. response.content
2. response.text

~~~python
response.content # 응답 raw data (byte)
response.text # response.encoding으로 디코딩하여 유니코드 변환
~~~

.text를 이용하더라도 문자열이 깨져서 보일때가 있는데, 이는 잘못된 인코딩으로 디코딩 되었기 때문이다. 
**이럴땐 직접 인코딩을 지정해서 디코딩하도록 한다!**

~~~python
# 이미지 데이터일 경우 .content 사용
with open('example.jpg', 'wb') as f:
  f.write(response.content)
  
# 문자열 데이터일 경우 .text 사용
html = response.text
html = response.content.decode('utf8')

# 또는
response.encoding = 'euc-kr' # encoding을 오버라이트한 후
html = response.text # .text 실행
~~~

json 응답일 경우

1. json.loads(응답문자열)을 통해 직접 deserialize 수행
2. .json()함수를 통해 deserialize 수행
3. json 포멧이 아닐경우 JSONDecodeError 예외 발생

~~~python
import json

# 1번
obj = json.loads(response.text)

# 2번
obj = response.json()
~~~



### POST 요청

~~~python
response = requests.post('http:/httpbin.org/post')
~~~

~~~python
request_header = { .. }
get_params = { .. }

# custom header 와 get 인자 지정
response = requests.post(url, headers=request_header, params=get_params)
~~~

post 요청은 **data** 또는 **files**이다

~~~python
# data인자와 file 인자로 post요청을 날린다
response = requests.post(url, data=..., files=...)
~~~



### JSON POST 요청

json api 호출 시 사용

~~~python
import json
json_data = {
  'k1': 'v2', 
  'k2': [1, 2, 3], 
  'text': '안녕'
}

# json포멧을 문자열로 변환 후, data인자로 지정
json_string = json.dumps(json_data)
response = requests.post(url, data=json_string)

# json 인자를 지정
# 내부적으로 json.dumps 처리
response = requests.post(url, json=json_data)
~~~

 

### 파일 업로드 요청

~~~python
# multipart/form-data 인코딩
files = {
	'first': open('f1.jpg', 'rb'), # 데이터만 전송
    'second': ('f2.jpg', open('f2.jpg', 'rb'), 'image/jpeg', {'Expires': '0'}), # 추천
}
post_params = {'k1': 'v1'} 
response = requests.post(url, files=files, data=post_params)
~~~

