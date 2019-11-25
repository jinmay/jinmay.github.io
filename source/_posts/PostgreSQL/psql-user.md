---
title: PostgreSQL의 사용자 관련 명령어
tags:
  - postgresql
  - psql
  - db
categories:
  - postgresql
date: 2018-11-11 17:52:40
---

# postgresql 사용자 관리

psql에서 사용자 관리에 대한 명령어에 관해서 알아보자. 

> 아래의 모든 명령어는 psql에 접속한 상태여야 한다.

### 사용자 생성

```postgresql
CREATE USER <생성할 유저 이름> password '<패스워드>';
```



### 사용자 역할(role) 또는 비밀번호 변경

```psql
ALTER USER <유저 이름> with password '<패스워드>';
ALTER USER <유저 이름> with superuser;
ALTER USER <유저 이름> with createrole;
```



### 사용자 권한 주기

```psql
GRANT all privilege on database <데이터베이스명> to <사용자명>;
```



### 현재 사용자 조회

```psql
SELECT current_user;
```



### 모든 사용자 조회

```psql
\du
\du+ # description까지 출력
```



