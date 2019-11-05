---
title: macOS에 RabbitMQ 설치하기
categories:
  - django
date: 2019-10-10 23:02:31
tags:
---

# macOS에 RabbitMQ 설치하기

맥북에 RabbitMQ를 설치하는 과정을 정리해본다. 이미 웹상에 많은 자료가 있지만 시간이 조금 지난 문서들인 것 같아서 현재 최신 버전인 3.8을 기준으로 작성하려고 한다.

일단 RabbitMQ라는 소프트웨어는 **AMQP 프로토콜을 구현한 메세지 큐**이다. '큐'라는 단어에서 알수있듯이 무언가를 순차적으로 저장하는 역할을 한다. 이렇게 저장된 메세지를 다른 시스템에게 순서대로 전달해주는 일을 하며 이러한 역할을 하는 소프트웨어를 메세지 큐 또는 메세지 브로커라고 한다.

보통, **웹 환경에서 보다 나은 사용자 경험을 위해서 비동기 작업 또는 백그라운드 작업을 해야할 때 이용하며 Redis와 함께 자주사용되는 메세지 브로커 소프트웨어 중 하나이다.**

> 웹 환경에서 메세지 브로커를 사용하는 예제를 보면 RabbitMQ와 Redis에 대한 자료가 가장 많을 것이다.

#### 설치하기

일단 맥북의 homebrew를 업데이트 한다.
~~~sh
brew update
~~~

그리고 바로 rabbitmq를 설치해주면 된다. RabbitMQ는 Erlang이라는 언어로 작성되어 있는 오픈소스 소프웨어인데 인스톨하는 과정에서 Erlang 설치와 의존성 패키지들을 설치할 것이다. (보통의 brew와는 다르게 설치 시간이 좀 걸린다)

~~~sh
brew install rabbitmq
~~~

rabbitmq와 관련한 CLI tools을 사용하기 위해서 환경변수를 설정해주어야 하는데, 바로 이 부분이 과거와 비교했을때 바뀐 부분일 것이다. **rabbitmq 서버 스크립트와 관련 커맨드라인 툴은 /usr/local/Cellar/rabbitmq 디렉토리의 sbin 폴더에 설치가 되는데 /usr/local/opt/rabbitmq/sbin 의 경로를 통해 사용할 수 있다고 한다.** 이 경로가 PATH에 잡혀있지 않기 때문에 따로 명시해주어야 한다.

~~~sh
export PATH=$PATH:/usr/local/opt/rabbitmq/sbin
~~~

터미널을 재시작하거나 사용중인 쉘 설정파일을 source 해주면 rabbitmq-server로 시작하는 명령어를 사용할 수 있다.


---
[참고]
<https://www.rabbitmq.com/install-homebrew.html>