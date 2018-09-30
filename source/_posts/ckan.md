---
title: 우분투에 CKAN 설치하기
tags:
  - ckan
  - python
  - open data
categories:
  - ckan
date: 2018-09-30 18:59:48
---

# Installing CKAN

## CKAN이란?

설치를 하기에 앞서서, CKAN이 무엇인지 간단한게 알아보는 것도 좋을거 같다. **CKAN**이란 오픈 놀리지(Open Knowledge)가 개발 및 관리하는 **데이터 포털 구축을 위한 오프소스 소프트웨어 플랫폼**이다. 20여 개국 중앙 정부의 공식 데이터 발행 플랫폼으로 사용되며, 더 많은 지방 정부, 커뮤니티, 과학 및 기타 데이터 포털이 이를 바탕으로 동작하고 있다고 한다.

##### 그렇다면, 본적적으로 CKAN을 설치해보자.

CKAN을 설치하는 방법에는 세 가지가 있다. 

> 1. operating system package를 통한 설치
> 2. source를 이용한 설치
> 3. docker compose를 이용한 설치

가장 쉽고 빠르게 설치할 수 있는 방법은 첫 번째 방법이다. 그러나 우분투 16.04 64비트 또는 우분투 14.04 64비트를 필요로 하는 제약조건이 있다. 따라서 CentOS와 같은 우분투를 제외한 다른 리눅스 배포판에 설치를 하려면 두 번째 방법을 통해 설치해야만 한다. 이번 글에서는 첫 번째 방법으로 설치를 할 것이며, 설치환경은 아래와 같다.

> Ubuntu 16.04 LTS 64bit - AWS의 EC2에서 임대받은 가상서버

## 설치

CKAN을 설치할때에 필요한 소프트웨어이다. 각 소프트웨어들이 무엇을 위해 사용되어지고 몇 번 포트를 사용하는지 익혀두고 진행하면 좀 도움이 많이 되는 듯 싶다.

| Service    | Port | Used for   |
| ---------- | ---- | ---------- |
| NGINX      | 80   | Proxy      |
| Apache2    | 8080 | Web Server |
| Solr/Jetty | 8983 | Search     |
| PostgreSQL | 5432 | Database   |
| Redis      | 6379 | Search     |



### 1. CKAN package 설치

1. 터미널을 열고, 먼저 apt를 업데이트 해준다

~~~shell
sudo apt-get update
~~~

2. 그리고 필요한 패키지들을 설치한다(git은 CKAN 확장프로그램을 위해 설치)

~~~shell
sudo apt-get install -y nginx apache2 libapache2-mod-wsgi libpq5 redis-server git-core
~~~

두 번째 과정을 진행하다보면 높은 확률로 한번에 설치가 안된다. 아파치가 돌아가고 있기때문에 정지시키고 재설치하자.

~~~shell
sudo service apache2 stop # 아파치 정지
sudo apt-get install -y nginx apache2 libapache2-mod-wsgi libpq5 redis-server git-core
~~~

3. CKAN 패키지 다운로드 및 인스톨

패키지를 다운로드 받는다. 현재 사용하고 있는 우분투의 버전을 확인하여 다운받고 인스톨까지 진행한다

~~~shell
# for 16.04
wget http://packaging.ckan.org/python-ckan_2.8-xenial_amd64.deb
sudo dpkg -i python-ckan_2.8-xenial_amd64.deb

# for 14.04
wget http://packaging.ckan.org/python-ckan_2.8-trusty_amd64.deb 
sudo dpkg -i python-ckan_2.8-trusty_amd64.deb
~~~



### 2. PostgreSQL 설치 및 설정

psql를 설치한다. **sqlalchemy.url** (/etc/ckan/default/production.ini)을 상황에 맞게 수정한다면 psql과 ckan은 서로 다른 서버에 위치해도 괜찮다.

~~~shell
sudo apt-get install -y postgresql
sudo -u postgres psql -l # 설치 확인
~~~

db의 인코딩이 utf8인지 확인하는 것도 좋다. ckan 설치를 계속하기 이전에 인코딩을 확인하고 넘어가자.

~~~shell
psql my_database -c 'SHOW SERVER_ENCODING' # from command line
SHOW SERVER_ENCODING # within psql
~~~

psql에 사용중인 계정이 없다면 새로 생성해주자. ckan_default라는 이름의 계정을 생성하며 적당한 pw를 입력하자

~~~shell
sudo -u postgres createuser -S -D -R -P ckan_default
~~~

계정을 생성해 주었으면 데이터베이스도 만들자

~~~shell
sudo -u postgres createdb -O ckan_default ckan_default -E utf-8
~~~



### 3. Solr 설치 및 설정

psql과 마찬가지로 /etc/ckan/default/production.ini의 solr_url를 상황에 맞게 수정한다면 ckan과 다른 서버에 설치해도 무방하다. solr을 설치하기 위해 아래의 명령어를 입력하자.

~~~shell
sudo apt-get install -y solr-jetty
~~~

1. jetty 설정

Jetty의 설정파일의 일부분을 수정해주어야 한다. 파일은 /etc/default/jetty8에 위치해있다.

~~~shell
NO_START=0            # (line 4)
JETTY_HOST=127.0.0.1  # (line 16)
JETTY_PORT=8983       # (line 20)
~~~

host의 경우 주의깊게 설정해주어야 한다. standalone으로 테스트를 위해 설치하는 것이라면 루프백 주소를 넣어도 상관없으나 지금 나의 경우는 외부 접속을 위해 AWS에서 제공해주는 EIP를 입력했다.

2. 서버 재시작

jetty의 설정을 변경했다면 서버를 재시작 / 시작 하자.

~~~shell
sudo service jetty8 restart # for 16.04
sudo service jetty restart # for 14.04
~~~

3. 확인

http://host_ip/solr/ 를 접속해보면 Solr의 welcome page를 볼 수 있다.

4. schema.xml 수정

기본 schema.xml 파일을 ckan schema 파일로 심볼릭 링크를 걸어준다.

~~~shell
sudo mv /etc/solr/conf/schema.xml /etc/solr/conf/schema.xml.bak
sudo ln -s /usr/lib/ckan/default/src/ckan/ckan/config/solr/schema.xml /etc/solr/conf/schema.xml
~~~

그리고 Solr을 다시 시작한다.

~~~shell
sudo service jetty8 restart # for 16.04
sudo service jetty restart # for 14.04
~~~



### 4. 설정파일 업데이트 및 데이터베이스 시작

1. ckan의 config 수정

아래의 코드는 예시이다. site_id에는 유니크한 값이 들어가야한다. 또한 site_url에도 실제 url을 적어준다.

~~~shell
# For example
ckan.site_id = default
ckan.site_url = http://demo.ckan.org
~~~

2. ckan db 시동

~~~shell
sudo ckan db init
~~~



### 5. Apache / Nginx 재시작

아파치와 엔진엑스를 재시작하자. 서버 재시작후 접속해보자.



##### 6. 마무리

맨 밑에 출처에도 있지만 [ckan 공식문서](https://docs.ckan.org/en/2.8/maintaining/installing/install-from-package.html)를 보게되면 정말 자세하게 안내하고 있는 것을 볼수 있다. 본인이 직접 설치하면서 추가한 내용도 조금은 있지만 웬만한 내용은 다 문서에 있으니 문서를 두 세번 읽어 보는 게 좋을거 같다고 느꼈다. 또한 본인만의 방법으로 정리하는 것도 좋을 거 같다.



--------------------------------

https://docs.ckan.org/en/2.8/maintaining/installing/install-from-package.html