---
title: PostgreSQL에 CSV 파일 임포트하기
tags:
  - psql
  - csv
  - postgresql
categories:
  - postgresql
date: 2018-11-15 11:41:43
---

# psql에 csv파일 임포트하기

갑자기 psql에 데이터를 업로드할 일이 생겼다. 간단할 것이라고 생각했으나 **local / remote**에 따라 해야하는 것도 달랐고 신경써야 할 부분이 있었다. ~~물론 처음 해보는 것이라서 그랬을 것 같단 생각...~~

### 기본 명령어

일단, local 환경이던 remote 환경이던 신경쓰지 않고 기본적인 file import  명령어를 보자.

```sql
COPY <table_name>
FROM <file_path>
WITH DELIMITER ',' csv HEADER
```

위와 같은 방법으로 사용했다.

**DELIMITER**는 CSV파일 내에서 어떠한 구분자가 쓰였는지 적어준다. 또한 **HEADER**은 CSV 파일 맨 윗줄은 데이터가 아닌 column 이라고 명시해 주는 것이다.



### 동일 머신

같은 머신 내에서는 **기본 명령어**에서 하듯이 똑같이 하면 된다. 하지만 문제는 서로 다른 머신에 있을때에 있다.



### 다른 머신

이번에 겪었던 문제는 다음과 같은 어려움이 있었다.

> CSV 파일은 로컬환경에 있다.
>
> 데이터를 통채로 올리는 것이 아닌 CSV 파일로부터 읽어서 한 줄 한 줄씩 삽입하는 것.
>
> 클라이언트 프로그램인 pgAdmin4를 처음 사용해보는 것.

COPY 명령어를 pgAdmin4와 psql 콘솔에서 실행해 보았지만 여러 크고 작은 문제들로 인해서 잘 되지않았다. NULL값이 문자열로 인식되는 경우도 있었고 인코딩 문제도 있었다. 

어찌되었던간에, 

문제는 다음과 같이 해결했다.

```sh
# in shell console
# 파일이 저장되어 있는 로컬환경
psql -U <username> 
	-d <db_name>
	-h <remote_addr>
	-c "\copy <table_name> FROM <file_path> WITH DELIMITER ',' csv HEADER;"
```

-c 옵션을 통해서 psql 콘솔 접속 시, 입력된 명령어를 실행하게 했다. 또한 로컬에서 원격으로 접속하는 것이기 때문에 -h 옵션으로 psql이 설치된 서버의 주소를 적어주었다.





