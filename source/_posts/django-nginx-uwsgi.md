---
title: Nginx와 uWSGI를 이용하여 Django 배포하기
tags:
  - django
  - nginx
  - uwsgi
  - 장고 배포하기
categories:
  - django
date: 2019-05-02 00:34:18
---

# Nginx와 uWSGI를 이용하여 Django 배포하기

> 웹 서버 Nginx와 uwsgi를 통해 Django를 배포하는 과정을 정리해본다.

### 엔진엑스와 아파치

Nginx는 웹 서버 중 하나로써 아파치 웹 서버의 C10K문제(한 시스템에 동시접속자 수가 1만명 이 넘어갈때의 효율적인 처리 방법)를 해결하기 위해서 Event-Driven 구조로 되어있는 웹 서버이다.

이벤트 드리븐 방식으로 동작한다는 것은 한 개 또는 고정된 프로세스만을 생성하고 그 프로세스 내에서 비동기 방식으로 요청을 처리하는 것을 의미한다고 한다. 따라서 동시 접속 요청이 많아지게 되더라도 프로세스나 스레드를 생성하는 비용이 존재하지 않기 때문에 보다 더 효율적으로 처리를 할 수 있다고 한다.

* Apache
  * 스레드 및 프로세스 기반 -> HTTP 요청 하나가 스레드 하나에 의해서 처리
  * 동시 요청이 많으면 많은 스레드가 생성되며 메모리 및 CPU의 낭비가 심해짐

* Nginx
  * 비동기 Event-Driven 기반 구조 -> 많은 요청을 효과적으로 처리
  * 아파치에 비해 적은 스레드로 클라이언트 요청 처리 가능


### 연동하기
본격적으로 nginx와 uwsgi 그리고 Django를 연동해보자. 아래와 같은 구성을 가지게 될 것이다.
~~~text
Client <-> Nginx <-> uWSGI <-> Django
~~~

#### 엔진엑스 설정
시스템에 nginx가 설치되어 있다면 **환경설정을 위한 파일들은 모두 /etc/nginx** 아래에 위치할 가능성이 높다. 엔진엑스는 여러 개의 가상 호스트를 지원하기 때문에 각 호스트 별로 다른 설정파일을 가질 수 있는데 이번에는 하나만 서빙하는 것을 목표로 정리하려고 한다.

> **myproject**라는 장고 프로젝트가 있고 기타 설정들은 모두 되어있다고 하자.

#### conf 파일 생성
**myproject를 위한 nginx conf를 생성해주어야 한다. 경로는 /etc/nginx/conf.d 아래에 위치**하며 위에서 말한 것 처럼 꼭 하나만 있어야 할 필요는 없다.(여러 호스트를 붙여서 사용하고 싶다면 여러개의 설정 파일이 존재할 수 있다.)

~~~shell
# cd /etc/nginx/conf.d
# vi myproject.conf

upstream myproject {
  # 둘 중 하나만 필요하다
  server 127.0.0.1:8000; # 웹 소켓 이용
  server unix:///home/ubuntu/myproject/myproject.sock; # 유닉스 소켓 이용
}

server {
  listen 80; # 80번 포트를 통해 접속 가능
  server_name <server_ip>; # 해당 서버의 주소
  charset utf-8;

  location / { # / (루트 경로)로 들어오는 주소는 upstream 으로 가게 된다
    uwsgi_pass myproject;
    include /etc/nginx/uwsgi_params; # uwsgi_params가 있는 파일 경로
  }

  location /static {
    alias /home/ubuntu/myproject/staticfiles; # 장고에서 STATIC_ROOT의 절대경로
  }
}
~~~
크게 보면 myproject.conf는 upstream 블록과 server 블록으로 나뉘어진다. 80번 포트로 요청이 들어오게 되면 uwsgi_pass에 적혀있는 myproject upstream으로 들어가게된다. nginx와 uwsgi를 연결해주는 부분에 유의해야 하는데 개인적으로 이 부분에서 조금 해맸던 것 같다.

#### nginx와 uwsgi
nginx와 uwsgi 사이에서 통신하는 방법에는 두 가지가 있다. **첫 번째 방법은 웹 소켓을 이용하는 것이고 두 번째 방법은 유닉스 소켓을 이용하는 것이다.** **웹 소켓을 이용하는 방법은 오버헤드가 비교적 크기 때문에 유닉스 소켓을 사용하는 것을 권장한다고 한다.**
~~~shell
upstream myproject {
  server unix:///home/ubuntu/myproject/myproject.sock; # 소켓이 위치할 경로를 명시하자
}
~~~
**upstream 블록에서 uwsgi와 통신할때 웹 소켓으로 할지 유닉스 소켓으로 할지 방법에 따라서 uwsgi를 실행하는 옵션이 달라진다.**

자세히 살펴보기 전에 nginx를 사용하지 않고 uwsgi - django를 배포할때의 uwsgi 옵션을 떠올려보자. 아마도 아래와 같은 옵션을 주었을 것이다.
~~~shell
uwsgi --http :8000 --module myproject.wsgi
~~~
클라이언트에서 접속하기 위해서 서버주소에 포트번호 8000번을 더했을 것이다. 

#### 웹 소켓 사용 시
웹 소켓을 사용해서 nginx - uwsgi 간에 통신을 한다면 아래와 같이 uwsgi의 옵션을 주어야 한다.
~~~shell
uwsgi --socket :8000 --module myproject.wsgi
~~~
**http 옵션이 아닌 socket 옵션을 주어야한다!!** 정말 주의해야한다. http 옵션을 주게되면 오랜시간 접속을 시도하다가 끊키게되며 방법을 찾는 데에 큰 어려움을 겪게 될 것이다. 

#### 유닉스 소켓 사용 시
유닉스 소켓을 사용한다면 소켓의 이름과 권한을 정확하게 설정해주어야 한다. 에러가 발생한다면 nginx의 에러로그(/var/log/nginx/error.log)를 보면서 해결책을 찾는 것이 큰 도움이 된다.
~~~shell
uwsgi --socket myproject.sock --chmod-socket=666 --module myproject.wsgi
~~~

위의 두 가지만 유의한다면 nginx + uwsgi + Django를 배포하는 데에도 큰 어려움을 없을 것 같다. 더 나아가서 해결해야할 것들이 있다면 uwsgi를 서비스에 등록하는 것이 있는데 이는 추후에 더 실습을 해보고 정리하자.