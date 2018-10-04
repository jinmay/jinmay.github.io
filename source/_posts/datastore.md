---
title: CKAN의 데이터스토어 확장기능 설치하기 및 테스트
tags:
  - ckan
  - extension
  - datastore
categories:
  - ckan
date: 2018-10-04 09:53:19
---


# CKAN DataStore

오픈 데이터포털 CKAN에 **DataStore**라는 extension을 설치해보자.

##### DataStore란 CKAN의 resource에서 구조화된 데이터를 저장하기위한 특별한 데이터베이스이다. 데이터는 CKAN의 resource에서 가져와서 DAtaStore에 저장할 수 있다.

CKAN의 리소스들이 DataStore에 저장되면, 이러한 것들을 할 수 있다: 

> 1. 리소스 페이지에서 자동으로 데이터 프리뷰를 제공한다
> 2. The DataStore API : 전체 데이터파일을 다운로드 하지 않고 API를 통해 데이터에 대해 search / filter / update 작업을 수행할 수 있다.

또한 DataStore는 DataPusher과 자주 사용되곤 하는데, DataPusher가 하는 역할은 FileStore 또는 Likned file에 의해 업로드된 CKAN의 리소스를 DataStore에 자동적으로 저장하는 것이다.

## 2. Setting up the DataStore

우선, DataStore를 설치하기 위해서 PostgreSQL 9.2 버전 이상을 필요로한다.

1. 플러그인 활성화

~~~shell
ckan.plugins = datastore
~~~

datastore를 ckan의 설정에 적어준다.

2. 데이터베이스 셋업

> 주의사항으로 퍼미션 설정해주는 부분은 신경써야한다. 권한이 제대로 주어지지 않을 경우 동작하지 않을 수 있으며 심각한 보안 이슈를 발생시킬 수도 있다.

DataStore는 데이터 저장을 위해 또 다른 PostgreSQL 데이터베이스를 필요로한다. 

~~~shell
sudo -u postgres psql -l
~~~

현재 존재하고 있는 데이터베이스를 위의 명령어를 통해 살펴볼 수 있다. 아마도 ckan 기본을 위한 ckan_default가 있을것이다. 설치때와 마찬가지로 인코딩이 utf8로 제대로 되어있는지 주의한다.

##### 유저와 데이터베이스 생성

DataStore를 위한 psql의 새로운 유저와 데이터베이스를 생성해야 한다.

datastore_default라는 이름을 가진 유저를 생성한다. 이 유저는 DataStore database에 대해서 read-only access만을 할 수 있는 권한을 가진다.

~~~shell
sudo -u postgres createuser -S -D -R -P -l datastore_default
~~~

datastore_default라는 이름을 가진 새로운 데이터베이스를 생성한다. 이 데이터베이스는 ckan_default에 소유여야 한다.

~~~shell
sudo -u postgres createdb -O ckan_default datastore_default -E utf-8
~~~



##### URL 설정

production.ini을 수정해야 한다. **ckan.datastore.write_url과 ckan.datastore.read_url의 주석을 해제한다**(uncomment). 그리고 자신의 환경에 맞게 수정한다. ex) 비밀번호와 접속ip 설정

~~~shell
ckan.datastore.write_url = postgresql://ckan_default:pass@localhost/datastore_default
ckan.datastore.read_url = postgresql://datastore_default:pass@localhost/datastore_default
~~~



##### 권한 설정

**db와 유저를 생성했으면, DataStore의 권한과 CKAN의 권한을 설정해야만한다.** CKAN의 paster명령어를 이용해서 할 수 있다(그러기 위해선 ckan의 가상환경을 activate해야 한다.)

만약 psql에 접속할 수 있으면 아래와 같은 명령어를 수행한다.

~~~shell
sudo -u postgres psql
sudo ckan datastore set-permissions | sudo -u postgres psql --set ON_ERROR_STOP=1
~~~

psql이 로컬에 설치되진 않았지만 ssh를 통해 접속할 수 있다면 아래와 같이 한다.

~~~shell
sudo ckan datastore set-permissions |
ssh dbserver sudo -u postgres psql --set ON_ERROR_STOP=1
~~~

psql를 할 수 없으면, 이렇게 하자

~~~shell
sudo ckan datastore set-permissions
~~~



3. 셋업 테스트

지금까지 잘 따라왔다면 셋업을 마무리됐을 것이다. 테스트하기 위해서 ckan을 재시작하고 아래의 명령어를 수행한다. (DataStore의 모든 리소스를 리스팅한다.) ckan 서버가 켜져있지 않다면 (paster command --config=/etc/ckan/default/production.ini) 을 통해 켜주고 해야 테스트가 제대로 실행된다.

~~~shell
curl -X GET "http://127.0.0.1:5000/api/3/action/datastore_search?resource_id=_table_metadata"
~~~

위의 명령어는 에러없이 JSON 형태의 페이지를 출력할 것이다.

쓰기 작업도 테스트하기 위해 새로운 DataStore 리소스를 생성해보자. 자신의 API KEY와 PACKAGE-ID를 적어주어 테스트해본다.

~~~shell
curl -X POST http://127.0.0.1:5000/api/3/action/datastore_create -H "Authorization: {YOUR-API-KEY}" -d '{"resource": {"package_id": "{PACKAGE-ID}"}, "fields": [ {"id": "a"}, {"id": "b"} ], "records": [ { "a": 1, "b": "xyz"}, {"a": 2, "b": "zzz"} ]}'
~~~

검색 작업은 다음과 같다. 마찬가지로 resource_id를 적어준다. : 

~~~shell
http://127.0.0.1:5000/api/3/action/datastore_search?resource_id={RESOURCE_ID}
~~~

삭제 작업도 해보자 : 

~~~shell
curl -X POST http://127.0.0.1:5000/api/3/action/datastore_delete -H "Authorization: {YOUR-API-KEY}" -d '{"resource_id": "{RESOURCE-ID}"}'
~~~



이상으로 DataStore의 API를 통해서 리소스 생성 / 검색 / 삭제 기능을 수행해보았다. ~~발생한 에러가 없다면 문제가 없다는 것이니 안심하도록 하자.~~

