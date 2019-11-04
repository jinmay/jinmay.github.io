---
categories:
  - django
title: django-celery-conf-systemd
---

# Celery 

지금까지는 로컬에서 테스트 하기 위해 **celery -A \<proj\> worker -l info** 를 통해서 실행했다. 수동으로 프로세스를 관리하지 않고 데몬으로 하기 위한 필요한 작업들을 정리하자.

간단하게 정리하자면

* Celery가 사용할 설정 파일 작성
* 데몬으로 실행하기 위한 service 파일 작성

이라고 할 수 있겠다.

## Celery 설정파일 작성하기

파일의 위치는 크게 상관 없다. 나의 경우 uwsgi의 설정파일 처럼 관리하기 위해서 /etc/celery/의 경로에 생성하고 사용하는 중이다.

~~~shell
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
~~~

/etc/celery/sites/ 폴더에 프로젝트 이름으로 된 conf 파일을 만들고 위와 같이 내용으르 작성했다. 


---
[참고]
<http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#init-script-celeryd>
<https://www.pincoin.co.kr/book/1/32/>
