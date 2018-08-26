---
categories:
  - elasticsearch
title: install-elasticsearch-in-centos
tags:
  - elasticsearch
  - centos7
---
# centos7에 엘라스틱서치 설치하기
ubuntu가 아닌 centos7에 엘라스틱서치를 설치하자. 엘라스틱서치는 자바8버전 이상을 필요로 하기때문에 자바가 서버에 설치되어있지 않다면 자바를 먼저 설치하고 온다. 자바 버전확인은 **java -version**을 입력하자.

## 설치하기
먼저 공식홈페이지에서 원하는 버전의 엘라스틱서치의 주소를 wget을 통해 받아온다. 이왕이면 rpm으로 한다.

~~~shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.0.rpm
rpm -i elasticsearch-6.3.0.rpm
~~~
