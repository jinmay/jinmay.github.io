---
title: 우분투에 RabbitMQ 설치하기
categories:
  - linux
date: 2019-10-11 10:48:49
tags:
---

# 우분투에 RabbitMQ 설치하기

우분투 16.04 버전에 RabbitMQ 설치하는 법을 정리해보자.

다른 일반적인 소프트웨어 처럼 apt를 통해서 바로 설치할 수 있지만, 최신 버전의 RabbitMQ를 설치할 수 없다. 때문에 최신 버전의 RabbitMQ를 이용하려면 아래의 두 가지 방법을 통해 설치하는 것이 추천된다.

* apt repository on Package Cloud or Bintray 사용하기(공식문서에서 추천하는 방법)
* dpgk -i를 통해 수동으로 설치

첫 번째 방법인 apt repo on Bintray를 통해 설치하는 법을 정리하려고 하고 Erlang/OTP가 설치되어 있어야 한다. 

### Install Erlang from an Apt Repostory on Bintray

> 참고: Erlang 설치하기 - [여기](https://jinmay.github.io/2019/10/11/linux-how-to-install-erlang-on-ubuntu/)

아마 Erlang을 설치했다면 **/etc/apt/sources.list.d/bintray.rabbitmq.list** 파일을 만들고 RabbitMQ 팀에서 관리하는 apt repo를 추가했을 것이다. 마찬가지로 같은 파일에 RabbitMQ를 위한 저장소를 추가해야 한다.

~~~shell
# vi in /etc/apt/sources.list.d/bintray.rabbitmq.list
# echo "deb https://dl.bintray.com/rabbitmq/debian {distribution} main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
# {distribution}에 각자의 환경에 맞는 우분투 버전명을 적어준다

echo "deb https://dl.bintray.com/rabbitmq/debian xenial main" | sudo tee /etc/apt/sources.list.d/bintray.rabbitmq.list
~~~

**추가된 저장소를 반영시키기 위해 apt 업데이트를 진행한 후 RabbitMQ를 설치하면 된다.**

~~~shell
sudo apt-get update -y
sudo apt-get install -y rabbitmq-server
~~~

### RabbitMQ 실행에 대한 간단한 설명

apt를 통해 설치를 진행했다면 관련 스크립트 및 서비스 또한 우분투 시스템에 자동으로 반영이 된다. **따라서 systemctl 커맨드를 통해 다른 소프트웨어처럼 사용할 수 있다.**

변경하지 않았다면 기본적인 포트번호는 5672를 사용한다. 그리고 기본 계정인 guest를 만들어주며 비밀번호도 guest이다. 기본적으로 제공해주는 guest 유저는 RabbitMQ가 localhost로서 동작할때만 권한을 가지게 되어 사용할 수 있다. **RabbitMQ를 단독으로 사용하려면 새로운 유저를 만들고 vhost까지 등록해주어야 사용할 수 있다!!**