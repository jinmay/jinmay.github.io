---
title: uwsgi와 함께하는 Django 배포하기
tags:
  - django
  - uwsgi
categories:
  - django
date: 2019-05-01 11:09:01
---

## uWSGI는 무엇일까?

python으로 웹 개발을 하려고하면 WSGI라는 걸 마주치기 마련이다. uWSGI란 WSGI라는 규칙을 따라서 만들어진 소프트웨어이며 **정적인 웹 서버(Apache / Nginx)와 python으로 작성된 Web Framework(Flask / Django) 사이의 통신을 도와주는 역할**을한다. 

그렇다면 WSGI라는 규칙은 무엇일까? 기본적으로 웹 서버는 HTTP 형식의 요청을 받아서 처리한 뒤 응답해주는 기능을 한다. 이와 같은 처리은 1차적으로 nginx를 통해 이루어지며 서버에서 처리해야 할 작업이 있다면 Django와 같은 WAS(Web Application Server)가 필요하다. 하지만 Django는 python으로 이루어져있기 때문에 HTTP 요청을 이해할 수 없는데 이때에 uWSGI와 같은 소프트웨어가 필요한 것이다. **즉, 파이썬 어플리케이션이 웹 서버와 통신하기 위한 명세라고 보면 된다.** 

> 즉, 파이썬 어플리케이션이 웹 서버와 통신하기 위한 명세가 WSGI인 것이다. 

uWSGI는 일종의 어플리케이션 컨테이너(Application Container)로써 동작한다고 볼 수 있다. 적재한 어플리케이션(Django)을 실행만 시켜주는 역할을 하기 때문이다. 

최종적으로 Django가 돌아가는 환경을 그려보면 아래와 같을 것이다.

~~~sh
Client <-> Nginx <-> uWSGI <-> Django
~~~



uWSGI와 WSGI에 대해서 알아봤으니 Django와 uWSGI를 연동하는 방법에 대해서 알아보자. 

#### 설치하기

uwsgi를 설치해야 한다. 파이썬 패키지 관리자로 설치 할 수 있다.

~~~sh
pip install uwsgi
~~~

uwsgi를 입력했을때 실행되었다가 바로 종료된다면 제대로 설치된 것이다.

#### Django 설정

Django 프로젝트를 생성하고 설정을 한다.

~~~sh
django-admin startproject myproject

# vi myproject/settings.py
ALLOWED_HOSTS=["*"]
~~~

Django 서버를 돌렸을때 접근가능한 호스트를 적어준다. 해당하는 IP를 명시적으로 적어도 되고 위와 같이 모든 주소를 받겠다는 표시를 해도 된다. runserver를 통해 제대로 동작하는지 확인한다.

~~~sh
python manage.py runserver
~~~



#### uWSGI 설정

uWSGI를 실행하여 Django와 연동해보자.

~~~sh
uwsgi --http :8001 --chdir /home/ubuntu/myproject --module myproject.wsgi
~~~

**--chdir** 옵션에 manage.py가 있는 Django 프로젝트의 경로를 적어준다. **--module** 옵션은 wsgi 파일을 입력해주면 되는데 프로젝트 이름과 같은 이름으로 적는다. 마지막 **--http** 옵션은 접속할 포트 번호를 적는다. 즉, 위와 같은 명령을 실행하면 서버의 아이피주소에 8001번 포트를 붙여서 접속할 수 있게된다. 

하지만 가상환경을 사용 중이라면 추가적인 옵션이 필요하다. 

~~~sh
uwsgi --http :8001 --virtualenv /home/ubuntu/.pyenv/versions/3.7.3/envs/myvenv --chdir /home/ubuntu/myproject --module myproject.wsgi
~~~

나머지 옵션은 동일하며 **--virtualenv** 옵션이 추가되었다. 사용중인 가상환경을 입력하자. 위 예시의 가상환경이름은 myvenv이다. uwsgi를 구동하기 위해서는 항상 위와 같이 번거로운 작업을 해야할까? **.ini 확장자의 설정파일을 만들어서 uwsgi 실행에 필요한 옵션들을 관리할 수 있다.**



#### uWSGI 설정파일 만들기

확장자는 ini이다. 하나의 머신에 여러 프로젝트를 사용할 수 있으므로 uWSGI를 위한 디렉토리를 만들고 설정파일을 모아서 관리하도록 한다.

~~~sh
mkdir -p /etc/uwsgi/sites
touch /etc/uwsgi/sites/myproject.ini
~~~

apt와 같은 패키지 매니저로 소프트웨어를 설치하는 것처럼 /etc 디렉터리 하위에 폴더를 생성해 주고 설정파일을 만들었다. 보통 어떤이름으로 만드는지 모르겠지만 Django 프로젝트와 같은 이름으로 생성해주었다.

~~~ini
# vi /etc/uwsgi/sites/myproject.ini
[uwsgi]
virtualenv = /home/ubuntu/.pyenv/versions/3.7.3/envs/myvenv
chdir = /home/ubuntu/myproject
module = myproject.wsgi
http = :8001
~~~

CLI 상에서 나열했던 옵션을 설정파일에 정의했다. 그리고 uwsgi를 실행할때 위의 옵션을 이용하여 구동하게 하면된다.



#### uWSGI 구동

~~~sh
uwsgi -i /etc/uwsgi/sites/myproject.ini
~~~

**-i** 옵션은 **--ini**의 약자이며 이전에 만든 설정파일을 이용해 uwsgi를 구동할 수 있게 해준다. uwsgi가 바로 꺼지지 않았다면 제대로 동작하는 것이다. 

지금까지 uWSGI와 Django를 연동하는 법을 알아보았다. uWSGI를 단독으로 사용하는 경우는 없다고 하며 앞 단에 Nginx와 같은 웹 서버를 두는 것이 일반적인 구성이라고 생각하면 될 것 같다. 그리고 현재까지 진행했던 방법으로는 부족한 면이 없지않아 있는데 그 이유는 uWSGI를 수동으로 작동시켜야 하기 때문이다. 이 와 같은 문제를 해결하려면 uWSGI를 구동하는 서비스를 등록해 주어야하며 나중에 정리하기로 하자.