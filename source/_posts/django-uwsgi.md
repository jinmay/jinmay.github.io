---
title: django와 uwsgi 연동하기 
tags:
  - django
  - uwsgi
  - wsgi
categories:
  - django
date: 2019-03-14 14:53:35
---

## uwsgi 사용해보기

nginx - uwsgi - django를 연동해서 배포하는 걸 AWS EC2를 통해 연습하고 있다. 그 중에서 어플레케이션 컨테이너라는 uWSGI를 사용하여 일단 nginx 없이 배포하는 걸 정리해보려고 한다.

uWSGI는 이름에서 보이듯이 WSGI라는 Web Server Gateway Interface의 역할을 한다. **웹 서버(Nginx, Apache)와  웹 어플리케이션 서버(WAS)의 사이를 연결해주는 것이다.** django는 python으로 만들어진 프레임워크이기 때문에 nginx에서 들어오는 요청을 그대로 받아들일 수 없으며 uwsgi가 웹 서버와 WAS 사이에서 소통을 도와주는 것이다. 

> uWSGI는 WSGI 규격으로 만들어진 서버 중에 하나이며 Django와 자주사용되지만 **gunicorn**이라는 것도 있으니 참고하도록 하자.



### uWSGI 사용하기

uwsgi를 꼭 django와 같은 웹 프레임워크와 사용해야만 하는 것은 아니며 일반 python 파일을 통해서도 테스트를 해볼 수 있다.

#### 설치

pip를 통해 설치한다. 만약 시스템에 파이썬이 여러개 설치되어 있다면 파이썬 버전을 유의해서 살펴보자.

~~~ shell
pip install uwsgi
~~~



##### 테스트(with python file)

django와 연동하기 전에 python 파일과 연동해서 테스트를 해보자

~~~python
# vi test.py
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"] # python3
~~~

위와 같이 test.py 파일을 생성한다. 그리고 uwsgi를 실행하기 위해 아래와 같이 쉘에 입력하자.

~~~shell
uwsgi --http :8001 --wsgi-file test.py
~~~

물론 test.py 파일이 있는 경로에 따라 알맞게 작성해주어야 한다. 두 가지 옵션을 넣었으며

> --http :8001 => 8001번 포트로 연결
>
> --wsgi-file => test.py 파일과 연결

와 같은 의미로 해석되어 작업을 수행한다. 현재 작업을 위해 연결되어 있는 컴포넌트들을 정리하면 아래와 같다. 

~~~shell
web client <-> uwsgi <-> Django
~~~



##### Django과 연결

python 파일 말고 django 웹 서버와 연결하려면 기본적으로 제공해주는 wsgi.py를 이용하면 된다. 이 때에 옵션은 --wsgi-file이 아닌 --module을 주면 된다.

~~~shell
# uwsgi --http :<포트번호> --module <프로젝트명>.wsgi
uwsgi --http :8001 --module myproject.wsgi
~~~

웹 브라우저로 8001번 포트에 접속해서 제대로 동작하는 지 살펴보자. 제대로 동작하고 있다면 아래와 같은 순서로 통신을 하고 있는 것이다.

~~~shell
web clien <-> uwsgi <-> django
~~~



지금까지 가상환경을 실행하지 않은 상태에서 uwsgi를 쓰는 법을 알아보았다. 가상환경을 사용한다고해서 크게 바뀌는 부분은 없지만 다른 유용한 옵션에 대해서 알아보자.

##### uwsgi의 유용한 옵션들

~~~shell
uwsgi --http :8001 --home /home/test_user/virtualenv/test --chdir /home/test_user/myproject --module myproject.wsgi
~~~

--home과 --chdir 옵션이 추가되었다. 각 옵션들의 의미는 다음과 같다.

> --http: 포트 번호 지정
>
> --home: 가상환경 디렉토리 지정
>
> --chdir: django 프로젝트의 경로
>
> --module: django 프로젝트의 wsgi 지정

직접 쉘에서 uwsgi 명령어에 옵션을 추가해 주면서 실행하는 방법을 살펴봤는데 쉘에 길에 적어야 한다는 불편함이 존재한다. wsgi의 옵션들을 정리해 놓은 ini파일을 만들어서 실습해보자.



##### uwsgi를 위한 설정파일

하나의 서버에 여러 프로젝트를 실행할 수 있으므로 uwsgi를 위한 디렉토리를 생성한다.

~~~shell
mkdir -p /etc/uwsgi/sites
~~~

그리고 프로젝트 이름과 동일하게 설정파일을 추가하자.

~~~shell
# cd /etc/uwsgi/sites
# vi myproject.ini
[uwsgi]
home = /usr/bin/python3 # 가상환경을 사용하고 있다면 가상환경의 경로를 적어준다.
						# home 옵션을 따로 적어주지 않으면 시스템에 깔려있는 파이썬을 사용한다
chdir = /home/test_user/myproject # Django 프로젝트의 경로
module = myproject.wsgi # Django 프로젝트를 생성하면 같은 이름으로 wsgi 모듈을 생성하기 때문에 
						# 프로젝트 이름과 동일하게 적어주면 된다.
http = :8001

socket = /home/test_user/myproject/myproject.sock # 소켓 파일이 저장될 경로와 이름 지정
chown-socket = test_user:test_user
chmod-socket = 660
vaccum = true # uwsgi 정지시에 자동으로 소켓 파일 삭제

master = true
processes = 1
~~~

정상적으로 저장이 되었다면 /etc/uwsgi/sites 폴더 아래에 myproject.ini 파일이 생성되었을 것이다. 이 설정파일로 uwsgi를 실행하자.

~~~shell
uwsgi -i /etc/uwsgi/sites/myproject.ini
~~~

8001번 포트로 접속해서 제대로 동작하는 지 확인하자!!