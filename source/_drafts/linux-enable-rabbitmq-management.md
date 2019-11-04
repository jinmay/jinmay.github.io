---
categories:
  - linux
title: linux-enable-rabbitmq-management
---

# 우분투에서 RabbitMQ의 management 플러그인 사용하기

RabbitMQ에서 사용할 수 있는 플러그인 중에서 브라우저를 통해 관리모듈을 사용할 수 있는 플러그인을 활성화 해보자.

일단 RabbitMQ의 플러그인에 대한 모든 명령어는 **rabbitmq-plugins**로 시작한다. 관리자 권한이 필요하다면 sudo를 사용하자.

#### 플러그인 목록 확인
 
~~~shell
sudo rabbitmq-plugins list

# [  ] rabbitmq_peer_discovery_k8s       3.8.1
# [  ] rabbitmq_prometheus               3.8.1
# [  ] rabbitmq_random_exchange          3.8.1
# [  ] rabbitmq_recent_history_exchange  3.8.1
# [  ] rabbitmq_sharding                 3.8.1
# [  ] rabbitmq_shovel                   3.8.1
# [  ] rabbitmq_shovel_management        3.8.1
# [  ] rabbitmq_stomp                    3.8.1
# [  ] rabbitmq_top                      3.8.1
# [  ] rabbitmq_tracing                  3.8.1
# [  ] rabbitmq_trust_store              3.8.1
# [  ] rabbitmq_web_dispatch             3.8.1
# [  ] rabbitmq_web_mqtt                 3.8.1
# [  ] rabbitmq_web_mqtt_examples        3.8.1
# [  ] rabbitmq_web_stomp                3.8.1
# ...
~~~

현재 사용할 수 있는 모든 플러그인을 출력한다.

#### management 플러그인 활성화

~~~shell
# sudo rabbitmq-plugins enable <플러그인 이름>
sudo rabbitmq-plugins enable rabbitmq_management
~~~

관리자 페이지 플러그인을 활성화한다. 기본 포트번호는 15672이고 어드민 계정을 생성해야 한다.

#### 어드민 계정 생성

RabbitMQ의 기본 명령어는 모두 **rabbitmqctl 로 시작**하며, 현재 어떠한 유저가 있는지 그리고 새로운 유저를 생성해보고 관리자 권한을 할당해보자.

~~~shell
# 유저 리스트 출력
sudo systemctl list_users

# 유저 생성
# 아이디/비밀번호 : admin/admin
sudo systemctl add_user admin admin

# 관리자 권한 할당
# set_user_tags <유저명> administrator
sudo systemctl set_user_tags admin administrator
~~~

생성한 어드민 유저로 로그인을 하고 관리 콘솔을 살펴 볼 수 있다!