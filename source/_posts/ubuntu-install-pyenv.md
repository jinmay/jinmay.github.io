---
title: ubuntu에서 pyenv 설치하기
tags:
  - linux
  - ubuntu
  - pyenv
categories:
  - linux
  - ubuntu
date: 2018-12-27 14:47:22
---

# pyenv 설치하기

동일한 파이썬 버전아래서 virtualenv를 통해 가상환경만 격리하는 방식을 사용해왔다. 하지만 파이썬 3.7 버전이 나온 이후로 파이썬 버전도 관리해야 할 필요성을 느껴서 pyenv를 설치하고 사용하고 있는 중이다. pyenv 설치가 어려운 편은 아니지만 설치 과정을 남겨본다.

### pyenv 설치

git을 통해서 설치할 수 있다. 

~~~shell
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
~~~

### 환경변수 설정

shell에서 pyenv 명령어를 바로 사용할 수 있게 환경변수를 세팅해주는 것이 편하다. 각자 사용하는 shell에 따라서 설정해주어야 한다.

~~~shell
# bash shell
vi ~/.bashrc

# z shell
vi ~/.zshrc
~~~

~~~shell
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
~~~

여기까지 문제 없이 했다면 pyenv 명령어를 사용할 수 있다. 하지만 pyenv를 통해 python을 설치하려면 몇가지 패키지 들이 필요한데 우분투를 사용한다고 가정하고 shell에서 다음의 명령어를 수행해준다.

~~~shell
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev
~~~

지금까지 우분투에 pyenv를 설치하는 과정을 알아보았다. 설치과정만큼 사용하는 법도 어렵지 않으며 같이 정리해 보기로 한다.



<hr>

# pyenv 사용법

### 설치가능한 파이썬 출력

거의 모든 버전을 지원하고 있다. pure python 뿐만 아니라 miniconda / anaconda 와 같은 파이썬도 지원함을 알 수 있다.

~~~shell
pyenv install -list
~~~

### 파이썬 설치

install 뒤에 버전을 입력해서 원하는 버전의 파이썬을 설치할 수 있다.

~~~shell
pyenv install <python_version>
~~~

### 설치된 목록 확인

현재 시스템에 어떤 파이썬들이 설치되어있는지 확인할 수 있다. system으로 라벨링 되어있는 건 현재 시스템이 default로 사용하고 있는 파이썬을 의미한다.

~~~shell
pyenv versions
~~~

### 파이썬 버전 선택

시스템에 설치된 파이썬 중에서 하나를 선택하여 사용하려고 할때 두 가지의 방법이 있다. **하나는 global하게 전역적으로 사용하며 다른 하나는 local로서 지정된 폴더에서만 사용하는 방법이다. **

~~~shell
# system global
pyenv global <python_version>

# locally
pyenv local <python_version>
~~~



<hr>

참고

http://blog.jeonghwan.net/2016/08/11/pyenv.html

