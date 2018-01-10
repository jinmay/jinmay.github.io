---
title: python과 PostgreSQL 연동하기
tags:
  - postgresql
  - python
  - database
categories:
  - archive
date: 2018-01-11 00:58:31
---


PostgreSQL은 **오픈소스 관계형데이터베이스(RDBMS)**이다. 오픈소스 진영의 RDBMS 중에서 두 번째로 큰 사용자층을 가지고 있으며 가장 많이 사용되는 DB는 MySQL이다. 처음 접해본 DBMS가 postgresql이었기 때문에 현재고 가끔식 사용하는 중이다.

이용중인 운영체제에 따라서 설치하는 방법이 조금씩 다르며, 같은 운영체제라고 해도 CLI와 GUI로 구분함에 따라 또 나뉘어진다.

## 설치
* in OSX
먼저 맥에서 어떻게 설치해야 하는지 알아보자. 공식홈페이지에서 배포하고있는 gui 기반의 소프트웨어도 있지만, 터미널에서 사용하기 위해 Homebrew를 통해 설치를 진행해본다.
~~~bash
brew update # 먼저 homebrew를 업데이트해준다
brew install  postgresql # psql 설치
~~~
  두 번재 명령을 통해서 psql을 설치할 수 있다. ~~설치는 매우 간단하다.~~

psql을 처음 설치했다면 db를 만들기 위해 아래와 같이 입력한다.
~~~bash
initdb /usr/local/var/postgres -E utf8
~~~

데몬을 통해 db서버를 항상 띄워놓을 수도 있지만 다음 명령어로 수동으로 실행하고 종료할 수 있다.
~~~bash
pg_ctl -D /usr/local/var/postgres start # 실행
pg_ctl -D /usr/local/var/postgres stop -s -m fast # 종료
~~~

## 기본 명령어
설치까지 완료하였고 python과 연동하기에 앞서서 가장 기초적인 명령어를 살펴보자.
* 클라이언트 접속
~~~bash
psql <db_name>
~~~
  클라이언트에 접속하기 위해서 psql이라는 명령어를 사용해야한다. 뒤에는 db명을 명시해야하며 비밀번호를 요구할 수도 있다.

* 데이터베이스 목록 확인
~~~postgresql
\l
~~~
  현재 데이터베이스의 목록을 출력하는 명령어는 \\l(역슬래시 + 영소문자 L)이다. 아마도 기본적으로 **postgres와 template0 그리고 template1**이 있을것이다. 또한 프롬프트를 통해 현재 어떠한 데이터베이스에 접속해 있는지도 알 수 있다.

* 데이터베이스 변경
~~~postgresql
\c <db_name>
~~~
  사용중인 데이터베이스를 변경한다. 명령어는 \\c(역슬래시 + 영소문자 C) 이다. 정상적으로 변경이 완료되었다면 프롬프트(쉘)의 이름이 변경된 것을 확인할 수 있다.

* 새로운 데이터베이스 생성
~~~postgresql
CREATE DATABASE <db_name>;
~~~
  새로운 데이터베이스를 생성하며 자동으로 로그인 중인 db를 변경해준다. 소문자로 적어도 작동은 하는 것 같고.. 문장 끝에 세미콜론을 잊으면 안된다.

* 스키마 목록 확인
~~~postgresql
\dn
~~~
  스키마 목록을 확인할 수 있다. 데이터베이스를 막 생성한 직후라면 기본적으로 public 밖에 없을 것이다.

* 테이블 목록
~~~postgresql
\dt
~~~
  현재 데이터베이스의 테이블 목록을 확인할 수 있다.

* 클라이언트 종료
~~~postgresql
\q
~~~
  psql 클라이언트를 종료한다.

* 테이블 정의
~~~postgresql
CREATE TABLE <table_name> (
  column1 datatype,
  column2 datatype,
);
~~~
  이름이 table_name 이고 데이터 타입이 datatype인 컬럼을 두 개 가진 테이블을 생성한다.

* 테이블 삭제
~~~postgresql
DROP TABLE <table_name>
~~~
  해당 테이블을 삭제한다. 해당 커맨드는 테이블 자체를 삭제하는 것이며, 테이블의 데이터를 삭제하려고 한다면 DELETE 또는 TRUNCATE 명령을 사용해야 한다.

* 테이블의 데이터 삭제
~~~postgresql
DELETE from <table_name>
~~~
 해당 테이블의 모든 데이터를 삭제한다.

지금까지 맥북에서 설치하는 방법과 간단한 명령어를 알아보았다. 

## python과 연동
간단하게나마 psql에 대해서 접해보았으니 원래 주제인 python과 연동하는 방법을 알아보자. 패키지 의존성 문제를 피하기 위해 virtualenv와 같은 가상환경을 이용하도록 하자.

실습을 위한 가상환경을 생성했으면 잊지말고 활성화를 하자. psql을 python에서 사용하려면 driver(adapter)를 이용해야만 한다. psql을 파이썬과 함께 사용할때 가장 많이 쓰이는 드라이버는 **psycopg2**이다. psycopg2란 파이썬을 위한 가장 알려진 psql db adapter 라고 공식문서에서 소개하고 있으며 주된 기능으로는 thread safety와 파이썬 db api 2.0을 완벽하게 수행하는 것이라고 한다.

* psycopg2 설치
~~~bash
pip3 install psycopg2
~~~
  매우 간단하다.

* 연동
~~~python
import psycopg2 # driver 임포트

# conn = psycopg2.connect("host=localhost dbname=test user=postgres password=pwtest port=5432")
conn = psycopg2.connect(host='localhost', dbname='test', user='postgres', password='pwtest', port='5432') # db에 접속
~~~
  먼저 설치한 psycopg2를 임포트한다. 그 다음으로 psql db에 접속할 차례인데 위의 예제코드처럼 두 가지 방법이 있다. 첫 번째는, 연결옵션들을 모두 하나의 따옴표("")안에 넣는 방법이고 두 번째 방법은 각각의 연결옵션마다 개별적으로 적어주는 방법이다.

  ~~~python
  cur = conn.cursor() # 커서를 생성한다
                      # 특정한 처리를 할때 사용함
  ~~~
    커서를 생성했다. 커서란 select 되는 특정 집합에 대해서 각 행에 대해 특정한 처리를 할 때 사용한다고 한다.(결과로 도출된 데이터에서 특정 위치, 특정 row를 가리킬 때 사용된다)

  ~~~python
  # 명령 실행 : 새 테이블 만들기
  cur.execute("CREATE TABLE test_table (title varchar, content text);") 

  # data 입력
  cur.execute("INSERT INTO test_table (title, content) VALUES (%s, %s)", ('hello' ,'qwerttyfdas'))
  ~~~
    생성한 커서를 가지고 명령을 수행하는 코드이다. execute()안에 psql 명령어를 입력하면 된다.

  **데이터를 집어넣는 명령을 수행할때 주의해야할 것들이 있다.**
  ~~~bash
  # example 1
  cur.execute("INSERT INTO numbers VALUES (%d)", (42,)) # WRONG
  cur.execute("INSERT INTO numbers VALUES (%s)", (42,)) # correct

  # example 2
  cur.execute("INSERT INTO foo VALUES (%s)", "bar")    # WRONG
  cur.execute("INSERT INTO foo VALUES (%s)", ("bar"))  # WRONG
  cur.execute("INSERT INTO foo VALUES (%s)", ("bar",)) # correct
  cur.execute("INSERT INTO foo VALUES (%s)", ["bar"])  # correct

  # exampel 3
  SQL = "INSERT INTO authors (name) VALUES (%s);" # Note: no quotes
  data = ("O'Reilly", )
  cur.execute(SQL, data) # Note: no % operator
  ~~~
    ex1 - 숫자형이라고 placeholder를 %d로 입력하지 않는다.
    ex2 - 튜플로 넘겨줄땐 항상 following comma가 필요하며 리스트로 넘겨주는 것 또한 가능하다.
    ex3 - 또한 .execute()안에 명령어를 직접 입력해주는 것 보다 수행할 sql문과 대입할 data을 따로 넘겨주는 것을 선호한다.

  ~~~python
  # test 테이블의 모든 데이터를 가져오고 출력한다
  cur.execute("SELECT * FROM test;")
  print cur.fetchone()
  
  # 데이터를 변경했다면 반드시 .commit() 해주어야 한다
  conn.commit()
  
  # 커서를 닫고 연걸을 종료한다.
  cur.close()
  conn.close()
  ~~~
    잊지말고 conn.commit()을 해주어야 삭제했던 변경했던 새로 데이터를 추가했던간에 실제 db에 반영이된다. ~~매번 commit을 하지 않아서 왜 db에 값이 저장되지 않을까? 하는 실수를 반복하고 있다.~~


--------
[참고]
<https://gist.github.com/xpepper/8110743>
<http://blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220847310053>
<http://www.sqler.com/400339>
<http://blog.secretmuse.net/?p=24>
<http://freeprog.tistory.com/100>