---
title: ckanext-harvest 설치하기
tags:
  - ckan
  - harvest
  - harvesting
  - harvester
categories:
  - ckan
date: 2018-10-29 10:09:01
---

# ckan harvesting 기능 설정

데이터 공유 플랫폼 CKAN에는 서로 다른 서버들간의 데이터를 주고 받을 수 있는 **harvest**라는 기능이 있다. 여러 익스텐션들이 존재하지만 CKAN에서 공식적으로 운영하고 있는 가장 기본이 되는 익스텐션을 설치하는 과정을 정리하려고 한다.

이 플러그인을 활성화하게 되면, CLI와 Web UI를 통해서 하베스팅 기능을 사용할 수 있다고 한다.

사전에 준비해야할 것으로는  데이터를 주고받을 모든 CKAN 서버는 최소 버전 2.0 이상을 사용해야 한다는 것이다.

## 설치

1. redis 설치

백엔드로 아래의 두 가지 소프트웨어를 사용할 수 있다. 

> Redis
>
> RabbitMQ

각자 사용하고 싶은 걸 사용해도 무방하지만, redis가 더 안정적이고 믿을 만 하다고 하기 때문에 CKAN org는 **redis**를 추천한다.

~~~sh
sudo apt-get update
sudo apt-get install redis-server
~~~

그리고 CKAN의 production.ini 설정파일에 아래의 설정을 추가해준다.

~~~ini
ckan.harvest.mq.type = redis
~~~

혹시 모르니 RabbitMQ도 정리해보자.

~~~sh
sudo apt-get update
sudo apt-get install rabbitmq-server

## in production.ini
ckan.harvest.mq.type = amqp
~~~

2. 가상환경 활성화

~~~sh
. /usr/lib/ckan/default/bin/activate
~~~

3. ckanext-harvest 패키지 다운로드

activate한 가상환경에 harvest를 위한 파이썬 패키지를 설치한다.

~~~sh
pip install -e git+https://github.com/ckan/ckanext-harvest.git#egg=ckanext-harvest
~~~

여기서 맨할 헤메곤 했는데, 그 이유는 pip의 권한 때문이었던 것 같다. 보통은 **python -m pip install <package_name>**을 하게 되면 무리없이 설치하곤 했는데 가끔씩 permission erorr를 내뱉는 경우가 있다. 

sudo를 사용하거나 python -m pip를 사용했는데도 permission error로 인해 설치하는데 어려움을 겪는다면 현재 내가 사용중인 pip의 python을 찾아서 이용해보도록 한다!!

4. 패키지 설치

위에서 다운받은 패키지의 폴더에 들어가 설치한다.

~~~sh
cd /usr/lib/ckan/default/src/ckanext-harvest
pip install -r pip-requirements.txt
~~~

5. 플러그인 추가

CKAN 설정파일에서 플러그인 harvest와 ckan_harvester를 추가한다.

~~~ini
ckan.plugins = ... harvest ckan_harvester
~~~

6. 필수 설정

가상환경을 activate 한 채로, 필수적인 테이블을 생성하기 위해 다음과 같은 명령어를 실행한다.

~~~sh
paster --plugin=ckanext-harvest harvestet initdb --config=/etc/ckan/default/production.ini
~~~

7. 재시작

~~~sh
sudo service apache2 restart
~~~

8. 접속

~~~text
http://ckan_url/harvest
~~~

