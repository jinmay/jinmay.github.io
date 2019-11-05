---
title: 시스템 어드민 계정 생성 및 테스트
tags:
  - sysadmin
  - ckan
categories:
  - ckan
date: 2018-10-04 10:41:44
---


# 시스템 어드민

#### ckan의 시스템 어드민은(sysadmin) 유저와 데이터셋을 관리하는 역할을 한다. 대부분의 기능은 web ui를 통해서 가능하지만 그렇지 않은 경우도 있다. ckan 페이지의 config file 수정과 같은 일은 cli를 통해서 서버에 접속해야만 사용할 수 있다.

> 시스템 어드민은 모든 조직에 접속할 수 있다. 유저의 정보도 확인 및 수정 가능하고 데이터셋 삭제도 가능하다. 때문에 sysadmin의 관리에 주의해야한다. 



## 시스템 어드민 계정 생성하기

어드민 계정을 생성하는 방법에는 **1. 새로운 어드민 계정 생성 2. 기존의 유저에 어드민 권한 부여** 두 가지가 있다. 

> paster 명령어를 사용하기 위해 가상환경을 activate 해야한다
>
> . /usr/lib/ckan/default/bin/activate
>
> cd /usr/lib/ckan/default/src/ckan

어드민 유저를 생성하기 위해 아래와 같은 명령어를 입력한다.

~~~sh
paster sysadmin add admin_test email=admin_test@example.com name=admin_test -c /etc/ckan/default/production.ini
~~~

email과 name을 입력할 수 있는데 알아서 적어준다. 참고로 나중에 user page에서 수정할 수 있다. 계정을 생성할때 비밀번호를 적어야 하는데 총 두번 입력하면 된다. 만약 이미 사용중인 계정에 어드민 권한을 부여하고 싶다면 아래와 같이 한다 : 

~~~sh
paster sysadmin add admin_test -c /etc/ckan/default/production.ini
~~~

##

### 테스트 데이터 생성

ckan이 잘 돌아가는 지 테스트 데이터 생성을 통해 알아볼 수 있다. 커맨드는 create-test-data 이다.

~~~sh
paster create-test-data -c /etc/ckan/default/production.ini
~~~


