---
title: ubuntu에서 pyenv 설치하기
tags:
  - ubuntu
  - pyenv
  - pyenv-virtualenv
  - autoenv
  - python
categories:
  - linux
date: 2019-03-16 00:04:44
---

## ubuntu에서 pyenv 설치하기

AWS를 통해서 ubuntu 16.04를 설치했는데 python3.4 버전이 깔려있었다. 패키지 버전 뿐만 아니라 파이썬 버전도 여러개를 사용하여 관리하기 위해서 pyenv를 설치하려고 한다. 

##### 사전 준비하기

ubuntu를 포함한 여려가지 리눅스 배포판에서 패키지 설치를 위해 거치는 build 과정에 발생하는 문제를 방지하고자 필요한 패키지들을 설치하자.

```sh
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev
```



##### pyenv 설치

pyenv를 설치하기 위해 git clone을 해준다.

```sh
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

그리고 쉘 설정파일에 적어주어야 하는 것이 있다. 각자 사용하고 있는 쉘의 종류에 따라 zshrc 또는 bashrc에 입력해주면 된다.

```sh
# vi ~/.zshrc
export PYENV_ROOT="$HOME/.pyenv
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

변경된 zshrc를 반영하기 위해 source를 한다.

```sh
source ~/.zshrc
```



##### pyenv를 이용해 python 설치

본격적으로 파이썬 설치를 해보자. pyenv를 이용하여 설치할 수 있는 파이썬의 리스트는 아래와 같이 확인할 수 있다.

```sh
# 설치 가능한 버전 리스트 출력
pyenv install --list

# python 3.7.2 설치
pyenv install 3.7.2

# 시스템에 설치된 파이썬 출력
pyenv versions

# 시스템 전역적으로 3.7.2 버전 사용
pyenv global 3.7.2
```



##### pyenv-virtualenv 사용하기

pyenv를 이용하여 여러 버전의 파이썬을 사용할 수 있게되었다. 가상환경도 사용하기 위해 pyenv-virtualenv로 사용해보자.

pyenv-virtualenv를 설치하기 위해서 다음의 순서로 작업을 진행한다.

```sh
git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
```

bashrc 또는 zshrc에 추가하고 source까지한다.

```sh
eval "$(pyenv virtualenv-init -)"
source ~/.zshrc
```

pyevn-virtualenv로 가상환경을 새로 만들기 위해서 아래와 같이 입력해준다.

```sh
# pyenv virtualenv <python_version> <virtualenv_name>
pyenv virtualenv 3.7.2 test-venv
```

파이썬 3.7.2 버전을 사용하는 가상환경 test-venv가 생성되었을 것이다. pyenv versions 명령어를 이용해서 현재 설치된 파이썬 뿐만 아니라 생성한 가상환경도 볼 수 있을 것이다.



##### autoenv 사용하기

autoenv를 이용하게 되면 프로젝트 폴더로 진입했을때 가상환경을 수동으로 activate할 필요 없이 자동으로 해준다. 방법은 아래와 같다. 일단 프로젝트에 사용할 가상환경이 만들어져 있어야 한다.

```sh
# cd myproject
pyenv local test-venv
```

프로젝트 폴더에 들어가서 위 와 같은 명령어를 입력하게 되면 가상환경이 활성화되고 폴더를 빠져나오게 되면 자동으로 deactivate까지 된다. 

