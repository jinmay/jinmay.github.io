---
title: GNU와 BSD의 find 명령어
tags:
  - macos
  - utility
  - freebsd
categories:
  - linux
  - command
date: 2019-01-25 17:36:51
---

# GNU의 find 명령어

쉘에서 find 커맨드를 통해서 어떤 경로의 파일명을 가져와야 할 상황이 생겼다. 간단하게 정의해보자면

> find 명령어는 디렉터리 계층에서 파일을 검색하기 위한 명령이다. 

와 같을 수 있다. 

find 명령으로 검색한 결과는 하나의 라인으로 된 파일명을 출력하며 흔히 사용하는 파이프를 통해 추가적인 작업을 할 수 있다.

#### 형식

~~~shell
find [검색할 디렉터리 경로] [옵션]

# example
find . -name "*.txt" // 현재 경로에서 .txt로 끝나는 모든 파일 출력
find ~/desktop -name "*.tet" // 사용자 홈 디렉터리/desktop 경로에서 .txt로 끝나는 모든 파일 출력
~~~

기본적으로 사용하기 위한 방법으로는 어렵지 않다. 하지만 macos를 사용중에 있어서 실행되지 않았던 옵션을 마주치게됐고 이것저것 찾아본 결과 **macos의 find 명령어는 GNU의 find와는 다른 것이 원인이었다.** 

BSD와 GNU 이러한 것들에 대해서 많이 들어보기는 했지만 자세하게 알고 있진 못하다. 어쨌든 결과적으로는 BSD의 특징을 조금 가지고 있는 macOS와 GNU 사이에는 분명히 차이가 존재하며 이러한 이유로 find 명령어를 제대로 쓸 수 없었던 것이었다.

macOS의 find 명령어가 GNU 소프트웨어인지 확인하기 위해서 **매뉴얼을 보기위한 man 명령어를 사용했다.** 그 결과는 

~~~shell
man find

# 결과
FIND(1) BSD General Commands Manual FIND(1)
...
...
...
...
~~~

와 같은 식으로 나오는 것을 확인할 수 있었다. **기존의 맥북에서 사용하는 find 명령어는 GNU가 아닌 BSD로 부터 온것임을 알 수 있다.** 

#### GNU's find 설치

GNU의 find 를 사용하기 위해서는 따로 설치를 해줘야하며 macOS의 homebrew를 통해 할 수 있다.

~~~shell
brew update
brew install findutils
~~~

설치완료 후 **gfind** 명령어를 통해 GNU의 find를 사용할 수 있을 것이다!

~~~shell
gfind / -name "*.txt" -printf "%f\n"
~~~

> GNU의 find 명령어가 아닌 BSD의 find를 사용한다면 -printf 옵션을 사용할때 에러가 난다. ~~사실 이부분 때문에 이와 관련된 부분을 찾아본 것이다.~~ 

