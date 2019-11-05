---
categories:
  - linux
title: linux-rabbitmq-commands-brief
---

# RabbitMQ의 간단한 명령어 정리

RabbitMQ를 apt를 통해 설치하면 커맨드 라인 툴인 rabbitmqctl이 같이 설치된다. 이 툴을 통해 RabbitMQ를 관리할 수 있다.

#### 시작 / 재시작 / 중지

```sh
sudo rabbitmqctl start / restart / stop
```

#### 상태 보기

```sh
sudo rabbitmqctl status
```

현재 시스템에 설치된 RabbitMQ의 상태정보를 보여준다.

### 계정 관련 명령어

#### 유저 목록

```sh
sudo rabbitmqctl list_users
```

생성된 유저의 목록을 출력한다. 유저의 태그도 같이 출력된다.

#### 유저 생성 / 삭제

```sh
# 생성
sudo rabbitmqctl add_user <유저명> <비밀번호>

# 삭제
sudo rabbitmqctl delete_user <유저명>
```

#### 유저에 태그 할당

```sh
sudo rabbitmqctl set_user_tags <유저명> <태그>

# management 플러그인 - 어드민 계정 만들기
sudo rabbitmqctl set_user_tags admin administrator
```

해당 유저의 태그를 설정한다. 15672 기본 포트를 사용하는 rabbitmq-management 플러그인에 접속 가능한 계정을 만들려면 administrator 태그를 주어야 한다.


---
[참고]
<https://teragoon.wordpress.com/2012/01/13/rabbitmqctl-%EC%82%AC%EC%9A%A9%EB%B2%95/>
<https://www.rabbitmq.com/rabbitmqctl.8.html>