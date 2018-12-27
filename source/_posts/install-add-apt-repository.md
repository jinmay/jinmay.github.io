---
title: ubuntu 16.04에서 add-apt-repository 설치하기
tags:
  - ubuntu
categories:
  - linux
  - ubuntu
date: 2018-06-19 00:46:28
---

### add-apt-repository 설치하기
로그 분석을 위해서 ELK 스택을 설치해보고자 한다. 현재 운영중인 식단알리미 말고 새로운 가상서버를 임대 받기에는 돈이 모자라서 어떻게 연습할 서버를 구하지 하던중, 최근에 잠깐 공부한 도커를 가지고 구현해보려고 한다. ubuntu 16.04를 설치했고 java8을 설치하기위해  add-apt-repository를 사용하려고 했는데 해당 명령어가 없다는 문제가 발생하였다. 그래서 검색을 하던 중 다음과 같은 해결방법을 찾았다.

~~~shell
apt-get install software-properties-common
~~~
위 와 같은 명령어를 이용하여 해당 패키지를 설치하면 **add-apt-repository**를 사용할 수 있다!!