---
title: vim에서 붙여넣기 할때 계단현상 방지하기
tags:
  - vi
  - command
  - linux
  - ubuntu
  - centos
categories:
  - linux
  - command
date: 2019-03-21 20:44:59
---

## vim에서 붙여넣기 할때 계단현상 방지하기

가끔씩 vim에서 붙여넣기를 하려고 하면 들여쓰기가 적용되지 않고 계단식으로 될때가 있다. 해결방법을 정리해보자.

##### 임시방편

붙여넣기를 하기 직전에 vi의 세팅을 변경해준다. 명령모드에서 설정값을 바꾸는 방법이다.

~~~shell
:set paste
~~~



##### vimrc 변경

~/.vimrc에서 설정한다.

~~~shell
# vi ~/.vimrc
set paste # 를 추가한다
~~~



