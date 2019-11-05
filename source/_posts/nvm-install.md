---
title: 맥북에 nvm 설치하기
tags:
  - javascript
  - es6
  - nvm
categories:
  - javascript
date: 2018-08-26 17:09:30
---

# macOS에 nvm(노드 버전 매니저) 설치하기
맥북에 node를 설치하려고 찾던중, 파이썬의 pyenv 처럼 노드 버전을 관리할 수 있는 툴이 있을 것 같아서 찾아보았다. nvm이 라는 툴이 있었고 brew를 이용하여 맥북에 설치하는 과정을 간단하게 정리해본다.

1. brew 업데이트 및 nvm 설치
언제나 그렇듯이 homebrew를 사용하기 전에 앞서서 업데이트를 해주고 nvm을 설치한다.
```sh
brew update
brew install nvm
```

2. 설치과정
nvm을 설치하면 이것저것 뭘 하라는 안내가 나오는데 디렉토리 생성과 쉘 설정을 추가하라는 내용이다. nvm을 위한 working directory가 없으면 생성하고 bashrc나 zshrc와 같은 현재 사용중인 쉘의 설정파일에 내용을 추가하는 것이다.
```sh
# 디렉토리 생성
mkdir ~/.nvm

# .zshrc 편집
export NVM_DIR="$HOME/.nvm"
. "/usr/local/opt/nvm/nvm.sh"
```

3. 마무리
터미널을 종료하고 다시 실행하거나 쉘을 재시작해주고 nvm이 제대로 설치 되었는지 확인해 본다.
```sh
# shell 재시작
source ~/.zshrc

# nvm 설치확인
nvm
```

지금까지 잘 따라왔다면 여러 노드환경을 사용하기 위한 준비는 끝났다. nvm의 기본 사용법에 대해서 알아보자.

## nvm
```sh
# 설치된 노드버전 출력
nvm list

# 노드 설치
# 원하는 노드 버전을 적어준다
nvm install (node_version)

# 노드 설치 - lts 버전
nvm install --lts

# 원하는 노드버전 사용
nvm use (node_version)

# 노드 사용 - lts 버전
nvm use --lts
```