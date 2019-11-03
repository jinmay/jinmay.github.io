---
title: 우분투에 Erlang 설치하기
categories:
  - linux
date: 2019-10-11 00:32:19
tags:
---

# 우분투에 Erlang 설치하기

> 우분투 16.04

우분투 16.04 버전에 Erlang 설치하는 과정을 정리해보자. Erlang을 사용하진 않지만 RabbitMQ가 erlang으로 되어있기 때문에 필수적으로 설치를 선행해야한다.

### Install Erlang from an Apt Repostory on Bintray

기존의 우분투 apt repo가 제공하는 Erlang/OTP은 최신 버전을 제공하지 않는 경향이 많기 때문에, RabbitMQ 팀이 지원하는 apt repo를 이용하여 설치하는 것을 추천한다. 

아래의 리눅스 배포판에서 사용할 수 있다.

* Ubuntu 18.04 (Bionic)
* Ubuntu 16.04 (Xenial)
* Debian Buster
* Debian Stretch

또한 이 apt repo는 아래의 Erlang 버전을 제공한다.

* 22.x
* 21.x
* 20.3.x
* 19.3.x
* master (23.x)
* R16B03 (16.x)

RabbitMQ 팀이 제공하는 apt repo를 사용하는 방법은 크게 아래의 네 가지 절차를 거친 후에 사용할 수 있다.

1. repository signing key를 임포트 함으로써 설치하는동안 apt가 package signatures를 확인한다
2. 새로운 repo를 위한 source list 추가
3. 패키지 메타데이터 업데이트
4. Erlang 설치

### 본격적인 설치

> 필요하다면 sudo 권한을 이용해서 설치를 진행하자

현재의 apt repo를 업데이트 한다.

~~~shell
sudo apt-get update -y
~~~

설치에 필요한 소프트웨어를 설치한다.

~~~shell
sudo apt-get install curl gnupg -y
~~~

RabbitMQ Signing Key를 설치한다
~~~shell
curl -fsSL https://github.com/rabbitmq/signing-keys/releases/download/2.0/rabbitmq-release-signing-key.asc | sudo apt-key add -
~~~

apt HTTPS transport를 설치한다
~~~shell
sudo apt-get install apt-transport-https
~~~

Bintray repositories를 추가한다
~~~shell
sudo vi /etc/apt/sources.list.d/bintray.rabbitmq.list

# bintray.rabbitmq.list 파일 안에 추가
# 사용중인 리눅스 배포판과 설치하려는 Erlang 버전을 뒤에 적는다
# deb https://dl.bintray.com/rabbitmq-erlang/debian $distribution $component
# deb https://dl.bintray.com/rabbitmq-erlang/debian xenial erlang-22.x
deb https://dl.bintray.com/rabbitmq-erlang/debian xenial erlang # 최신의 안정화 버전을 설치한다
~~~

참고로 \$distribution과 \$component는 자신의 환경에 맞게 설정하면 된다. 


* bionic for Ubuntu 18.04
* xenial for Ubuntu 16.04
* buster for Debian Buster
* stretch for Debian Stretch

추가된 apt repo를 반영하기 위해 apt update
~~~shell
sudo apt update -y
~~~

Erlang을 설치한다
~~~shell
sudo apt-get install -y erlang-base \
                        erlang-asn1 erlang-crypto erlang-eldap erlang-ftp erlang-inets \
                        erlang-mnesia erlang-os-mon erlang-parsetools erlang-public-key \
                        erlang-runtime-tools erlang-snmp erlang-ssl \
                        erlang-syntax-tools erlang-tftp erlang-tools erlang-xmerl

# rabbitmq-server와 의존성 패키지 설치
sudo apt-get install rabbitmq-server -y --fix-missing
~~~

에러없이 여기까지 왔다면 설치는 무사히 끝난다. 

---
[참고]
<https://www.rabbitmq.com/install-debian.html#apt-bintray-erlang>