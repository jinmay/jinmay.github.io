---
categories:
  - django
title: django-uwsgi-service-registration
tags:
  - django
  - uwsgi
  - ubuntu
  - service
---
# ubuntu 16.04 LTS에 uwsgi 서비스 등록하기

nginx + uwsgi + django를 배포하다보면 uwsgi를 수동으로 구동하곤한다. 단지 배포과정을 공부하는 입장에서는 이 정도면 충분하다고 생각했다. 하지만 문제점이 있었는데 서버를 재부팅하면 nginx는 저절로 구동되지만 uwsgi는 그렇지 않았다. 그리 복잡하지 않았기에 수동으로 돌려주고 터미널을 끄는 식으로 하곤했지만 현재 운영하고 있는 서비스의 안정성을 위해서 systemd에 등록하는 방법으로 변경했던 과정을 정리한다.


### uWSGI 자동실행하기

#### uWSGI 서비스 설정파일 작성
리눅스 서비스에 등록하기 위해서는 서비스 파일을 작성해야한다. 파일 경로는 /etc/systemd/system/uwsgi.service 에 하면된다.
~~~service
[Unit]
Description=uWSGI
After=syslog.target

[Service]
ExecStart=/root/uwsgi/uwsgi --ini /etc/uwsgi/sites/myproject.ini # ini의 경로로 입력
# Requires systemd version 211 or newer
RuntimeDirectory=uwsgi
Restart=always
KillSignal=SIGQUIT
Type=notify
StandardError=syslog
NotifyAccess=all

[Install]
WantedBy=multi-user.target
~~~
uwsgi의 systemd 공식문서를 참고했다. uwsgi를 미리 만들어준 ini파일과 실행하는 부분에 집중하자. 쉘에서 uwsgi를 실행하기 위해 입력하는 명령어와 다른 점은 uwsgi의 경로를 절대경로로 적어준 것이다. 그냥 uwsgi로만 적게되면 absolute path가 아니라는 에러가 발생하게 되는데 이게 주된 이유인지는 확실하지 않다. **어찌됐든 uwsgi가 설치된 절대경로를 통해 실행해야 한다는 것에 유의하자.**

~~~shell
# uwsgi가 설치된 절대경로와 옵션으로 입력할 ini 파일의 절대경로에 주의하며 적자.
ExecStart=/root/uwsgi/uwsgi --ini /etc/uwsgi/sites/myproject.ini
~~~

서비스 파일 작성을 마쳤다면 이 파일을 가지고 등록과정을 거치면 된다. 여러가지 방법이 있는 것 같은데 간단하게 등록할 수 있는 방법을 먼저 익혀보았다.

~~~shell
sudo systemctl start uwsgi.service # uwsgi.serivce 실행
sudo systemctl enable uwsgi # 심볼릭 링크 생성
~~~
enable 명령은 심볼릭 링크를 생성해주며 서비스에 등록되어 시스템을 재부팅하게 될 경우 uwsgi.service를 다시 실행시켜주는 것 같다.