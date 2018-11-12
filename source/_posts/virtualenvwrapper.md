---
title: virtualenvwrapper를 이용한 파이썬 가상환경 관리
tags:
  - python
  - virtualenv
  - 가상환경
  - virtualenvwrapper
categories:
  - python
date: 2018-11-12 14:31:57
---

# virtualenvwrapper 설정하기

만약 하나의 컴퓨터에 여러개 파이썬 프로젝트가 있다면 어떻게 해야할까? 사용해야하는 파이썬 버전이 동일하고 패키지의 버전마저 같다면 문제가 될 건 없다. 하지만 프로젝트마다 사용중인 패키지의 버전이 다르다면....? 문제는 꽤나 복잡해지게 된다. 

예를들어, 

A라는 서비스를 만들기 위해서 Djagno 1.11 버전을 사용하고 / B라는 서비스를 위해 Django 2.0을 사용한다면 어떻게 해야할까? 

왔다갔다 할때마다 설치 / 삭제를 반복해야 하는 것일까? 

 

그래서, 프로젝트 별로 관리를 하기 위해서 가상환경을 폴더 단위로 정할 수 있는 패키지들이 있다. 나의 경우 현재 **virtualenv**라는 패키지를 이용하고 있다. virtualenv를 이용하면 프로젝트 별로 가상환경을 설정할 수 있어서 A에는 django 1.11를 설치하고 B에는 django 2.0 을 설치해서 진행중인 프로젝트를 왔다갔다 할때마다 가상환경만 실행해주면 문제가 발생하지 않는다. 

하지만 여기에도 불편한 점이 있었으니.. 

바로 가상환경을 실행하기 위해서 source <virtualenv_name>/bin/activate를 수동으로 실행해주어야 하는 것이다. 폴더명을 항상 외울수도 없는 노릇이고.. 타이핑 하는 수고도 꽤나 많기 때문에 귀찮은 작업이 될 수 있는데, 이러한 문제점을 해결하기 위해서 **virtualenvwrapper** 라는 패키지를 사용하려고 한다. 

한마디로 얘기하자면

> 현재 위치한 경로와 상관없이 가상환경을 실행할 수 있다는 것이다!!!

------------------

## 설치

virtualenv는 이미 사용중이며, pip를 통해서 virtualenvwrapper를 설치해보자.

~~~shel
pip install virtualenvwrapper
~~~

그리고, 홈 디렉토리로 이동하여 가상환경들이 저장될 폴더를 만든다.

~~~shell
cd ~
mkdir .virtualenvs
~~~

홈 디렉토리의 쉘 설정에서 아래의 코드를 복사하여 넣어준다. (각자 사용중인 쉘 환경에 맞게 설정해 준다.)

~~~shell
# in ~/.zshrc
# in ~/.bashrc

export WORKON_HOME=~/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON="$(which python3)"
source /usr/local/bin/virtualenvwrapper.sh
~~~

저장하고 쉘을 재시작해주면 virtualenvwrapper를 사용할 수 있게된다.

만약 /usr/local/bin/virtualenvwrapper.sh 파일이 존재하지 않으면 아래 명령어로 파일을 찾아서 **source 부분을 대체해준다.**

~~~shell
find /usr -name virtualenvwrapper.sh
~~~





## virtualenvwrapper 명령어

기본적인 명령어를 알아보자.

* 가상환경 만들기

~~~shell
mkvirtualenv <virtualenv_name> # 이름
~~~

mkvirtualenv 명령어를 통해 가상환경을 새로 만들어 주면, 홈 디렉토리의 .virtualenvs 폴더에 저장된다.

* 가상환경 삭제

~~~shell
rmvirtualenv <virtualenv_name>
~~~

가상환경을 삭제한다. 삭제하는 방법으로 위의 코드처럼 **명령어를 이용하는 방식**과 **직접 폴더를 지우는 방식**이 있다.

* 가상환경 목록 출력

~~~shell
workon
~~~

아무런 인자 없이 **workon**을 입력하면 현재까지 생성한 가상환경의 목록을 알 수 있다.

* 가상환경 진입 / 빠져나오기

~~~shell
# 진입
workon <virtualenv_name>

# 빠져나오기
deactivate
~~~

