---
title: mac에 postgresql 설치하기
tags:
  - postgresql
  - psql
categories:
  - postgresql
date: 2019-02-16 19:33:19
---

# how to install postgresql on macOS

> 갑자기 psql를 업데이트하고 싶었다. 기존에 9.6버전을 사용하고 있었고 homebrew을 통해 업데이트 하려고하니 11버전으로 갈 수 있다고 한다. 그래서 업데이트를 했고 결과적으로 실패했다.. 덕분에 크리티컬하진 않지만 이전에 사용하던 것을 지우고 다시 설치를 했고 그 과정을 정리해본다.

**macOS를 사용중이라면 homebrew를 통해서 수월하게 설치할 수 있다.**

> **주의**
>
> psql를 upgrade한 뒤 이전의 DB를 migration 하는 과정이 아닌 새로 설치하는 과정이다. **업그레이드가 아닌 점을 분명히하자!!**

### 설치과정

##### 기존 psql 삭제

```shell
brew uninstall --force postgresql
```

##### psql 관련 파일 삭제

```shell
rm -rf /usr/local/var/postgres
```

이전에 brew를 통해서 psql을 설치했다면 위와 같은 경로에 관련 파일들이 위치해있을 것이다. 기존에 있던 psql에 대한 관련 파일이므로 같이 삭제하도록하자. 

_만약 brew uninstall 후 관련 파일 삭제를 하지 않고 다시 brew install을 하게 된다면 기존에 있던 파일들로 인해서 제대로 동작하지 않을 수도 있다._ 

##### psql 설치

```shell
brew install postgres
```

##### 서버 실행

~~~shell
pg_ctl -D /usr/local/var/postgres start
~~~

##### DB 생성

~~~shell
initdb /usr/local/var/postgres
~~~

때에 따라서 DB를 생성하는 과정에 아래와 같은 에러가 발생할 수 있다. 

_initdb: directory "/usr/local/var/postgres" exists but is not empty
If you want to create a new database system, either remove or empty
the directory "/usr/local/var/postgres" or run initdb
with an argument other than "/usr/local/var/postgres"._

이미 DB가 존재하며 empty하지 않기 때문에 생긴 에러이다. 삭제하고 다시 생성하면 되는 간단한 이슈이다.

##### old db 삭제

~~~shell
rm -r /usr/local/var/postgres
~~~

##### 다시 DB 생성 시도

~~~shell
initdb /usr/local/var/postgres
# 또는
initdb /usr/local/var/postgres -E utf8
~~~

##### 테스트 DB  생성

~~~shell
createdb test
~~~

##### psql 콘솔 접속 및 확인

~~~shell
psql test
~~~

<hr>

<참고>
<https://medium.com/@Umesh_Kafle/postgresql-and-postgis-installation-in-mac-os-87fa98a6814d>