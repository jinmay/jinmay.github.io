---
title: '[CKAN]DataPusher 설치하기'
tags:
  - ckan
  - datapusher
categories:
  - ckan
date: 2018-10-26 19:23:42
---

CKAN에 DataPusher를 설치하자. DataPusher란 CKAN에 자동으로 CSV / Excel 파일 로딩을 추가하는 역할을 한다. DataPusher를  이용하기 전에 DataStore가 함께 설치된 CKAN이 필요하다.

------------------------

> 2.2버전 이상부터) 만약 CKAN을 package install을 통해 설치 했다면, DataPusher를 이미 설치되어 있다.

기본적으로 DataPusher는 8800번 포트를 사용한다.

설치과정은 아래와 같다.

```sh
# DataPusher의 requirements 준비
sudo apt-get install python-dev python-virtualenv build-essential libxslt1-dev libxml2-dev git libffi-dev

# 가상환경 준비
sudo virtualenv /usr/lib/ckan/datapusher

# 소스 디렉터리 준비
sudo mkdir /usr/lib/ckan/datapusher/src
cd /usr/lib/ckan/datapusher/src

# 소스 clone
sudo git clone -b 0.0.14 https://github.com/ckan/datapusher.git

# DataPusher 밑 requirements 설치
cd datapusher
sudo /usr/lib/ckan/datapusher/bin/pip install -r requirements.txt
sudo /usr/lib/ckan/datapusher/bin/python setup.py develop

# 아파치 기본 설정파일 복사
# (만약 아파치 2.4이하 버전을 사용중이면, deployment/datapusher.apache2-4.conf 파일 사용)
sudo cp deployment/datapusher.conf /etc/apache2/sites-available/datapusher.conf

# 기본 DataPusher wsgi파일 복사
#(see note below if you are not using the default CKAN install location)
sudo cp deployment/datapusher.wsgi /etc/ckan/

# 기본 DataPusher 설정 복사
sudo cp deployment/datapusher_settings.py /etc/ckan/

# 8800번 포트 오픈
# 아래의 두 가지 명령 중 하나만 실행해야 한다!!
# 수동으로 수정하려면 /etc/apache2/ports.conf 파일 이용
sudo sh -c 'echo "NameVirtualHost *:8800" >> /etc/apache2/ports.conf'
sudo sh -c 'echo "Listen 8800" >> /etc/apache2/ports.conf'

# DataPusher Apache site 실행
sudo a2ensite datapusher
```

DataPusher를 설치하고 돌려보는 것 까지 진행했다. 이제 CKAN의 설정파일로 가서 플러그인을 추가해 준다.

```sh
ckan.plugins = ... datapusher
ckan.datapusher.url = 'real_url' # EIP와 localhost를 적어도 일단 둘다 가능함..!
```

설정파일까지 수정완료 했다면, 아파치를 재시작한다!