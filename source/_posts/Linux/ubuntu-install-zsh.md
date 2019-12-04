---
title: AWS EC2 ubuntu 16.04에 zsh 적용하기
tags:
  - ubuntu
  - zsh
  - oh-my-zsh
categories:
  - linux
date: 2019-03-17 00:16:29
---

## AWS EC2 ubuntu 16.04에 zsh 적용하기

> 맥에서 주로 zsh + oh-my-zsh 을 사용하고 있는데 생각난김에 사용하고있는 aws ec2 ubuntu에 zsh를 적용해보았다. 권한 문제를 비롯하여 맥에서 설치할때 겪지 못했던 에러발생했다. ubuntu에서 zsh 설치하는 여러가지 방법 중 이번에 정리할 방법은 꽤나 심플했고 권한문제도 발생하지 않았다.

##### apt 업데이트

AWS EC2 인스턴스를 열게되면 보통 패키지 업데이트를 먼저 해준다.

```sh
sudo apt update
```



##### zsh 설치

```sh
sudo apt install zsh
```



##### zsh 적용

지금까지 쉘을 변경하기 위해서 chsh 명령어를 사용해왔는데 어쩔땐 되고 어쩔땐 안되는 알 수 없는 일들이 많았다. 어떻게든 되게 만들었지만 이렇게 한다면 큰 문제없이 로그인 쉘을 변경할 수 있을 것이다.

```sh
# sudo vi /etc/passwd
# 로그인 계정의 log-in shell을 zsh의 경로로 변경해준다
# which zsh
/bin/bash -> /usr/bin/zsh
```



##### oh-my-zsh 설치 및 적용

zsh와 찰떡궁합인 oh-my-zsh도 같이 적용하자.

```sh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```



##### 끝

나갔다가 재접속하면 성공적으로 zsh을 사용할 수 있을 것이다!!