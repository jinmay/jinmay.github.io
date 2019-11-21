---
title: Celery 데몬으로 관리하기
categories:
  - django
date: 2019-11-11 22:03:44
---

지금까지는 로컬에서 테스트 하기 위해 **celery -A \<proj\> worker -l info** 를 통해서 실행했다. 수동으로 프로세스를 관리하지 않고 데몬으로 하기 위한 필요한 작업들을 정리하자.

간단하게 정리하자면

- Celery가 사용할 설정 파일 작성
- 데몬으로 실행하기 위한 service 파일 작성

이라고 할 수 있겠다.

## Celery 설정파일 작성하기

파일의 위치는 크게 상관 없다. 나의 경우 uwsgi의 설정파일 처럼 관리하기 위해서 /etc/celery/sites/의 경로에 생성하고 사용하는 중이다.

```sh
# vi /etc/celery/sites/proj.conf

# 노드, 워커 개수 (보통 한 개로 시작한다)
CELERYD_NODES="worker1"

# celery 명령어의 절대 경로 위치
# 가상환경 내에 설치된 경로인지 꼭 확인하자
CELERY_BIN="/home/ubuntu/.pyenv/versions/sample_proj/bin/celery"

# 앱 인스턴스 (예: Proj)
# Django에서 설정한 Celery app 이름을 확인하자
CELERY_APP="proj"

# manage.py 호출 방법
CELERYD_MULTI="multi"

# 워커로 전달할 추가 명령어 옵션
CELERYD_OPTS="--time-limit=300 --concurrency=2"

# - %n 노드 이름의 첫 부분
# - %I 현재 자식 프로세스 인덱스
#   prefork pool을 사용할 때 경쟁상태(race condition)을 피하기 위해 중요
CELERYD_PID_FILE="/tmp/celery-%n.pid"
CELERYD_LOG_FILE="/tmp/celery-%n%I.log"
CELERYD_LOG_LEVEL="INFO"
```

/etc/celery/sites/ 폴더에 프로젝트 이름으로 된 conf 파일을 만들고 위와 같이 내용으르 작성했다.

Celery의 설정 파일을 작성했다면 데몬으로 만들기 위해서 서비스 파일을 만들어야 한다.

## systemd로 만들기

위치는 마찬가지로 uwsgi.service가 있는 폴더에서 작업했다. 관리자 권한이 필요할 수도 있다.

```sh
# vi /etc/systemd/system/celery.service

[Unit]
Description=Celery Service
After=network.target

[Service]
Type=forking

# Celery의 설정파일 경로
EnvironmentFile=/etc/conf.d/celery

# 실행될 디렉터리 - app 프로젝트의 경로를 적어주면 된다
WorkingDirectory=/opt/celery

# 실제 구동할때 입력되는 명령어이다
# 이전에 생성했던 conf 파일의 설정을 통해 괄호 안에 값이 들어가게 된다
ExecStart=/bin/sh -c '${CELERY_BIN} multi start ${CELERYD_NODES} \
  -A ${CELERY_APP} --pidfile=${CELERYD_PID_FILE} \
  --logfile=${CELERYD_LOG_FILE} --loglevel=${CELERYD_LOG_LEVEL} ${CELERYD_OPTS}'
ExecStop=/bin/sh -c '${CELERY_BIN} multi stopwait ${CELERYD_NODES} \
  --pidfile=${CELERYD_PID_FILE}'
ExecReload=/bin/sh -c '${CELERY_BIN} multi restart ${CELERYD_NODES} \
  -A ${CELERY_APP} --pidfile=${CELERYD_PID_FILE} \
  --logfile=${CELERYD_LOG_FILE} --loglevel=${CELERYD_LOG_LEVEL} ${CELERYD_OPTS}'
StandardError=syslog

[Install]
WantedBy=multi-user.target
```

나는 conf 파일에서 사용하지 않았던 설정이 있었기 때문에 몇몇의 옵션은 삭제하고 파일을 만들었다.

Celery의 설정(conf)파일과 데몬(celery.service)파일을 만들었다면 데몬에 등록을 하고 구동하면 된다.

## 데몬 등록 및 구동

```sh
sudo systemctl enable celery
sudo systemctl start celery
```

## 데몬 재시작

```sh
sudo systemctl daemon-reload
sudo systemctl restart celery
```

celery.service 파일에 변경점이 있다면 deamon-reload를 해주고 재시작하자

지금까지 Celery를 데몬에 등록해서 사용하는 방법을 정리했다. 앞서서 RabbitMQ에 대해서도 정리를 간략하게 했지만 Celery는 메세지를 가지고 작업을 수행하기 때문에 RabbitMQ와 같은 메세지 브로커와 함께 사용되어야 한다는 것을 잊으면 안된다.

[참고]  
<http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#init-script-celeryd>
<https://www.pincoin.co.kr/book/1/32/>
