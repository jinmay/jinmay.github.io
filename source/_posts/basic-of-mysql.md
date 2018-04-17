---
title: MySQL 기본
tags:
  - database
  - mysql
categories:
  - mysql
date: 2018-04-17 23:44:05
---

# MySQL 기본
> 파이썬을 쓸때 주로 DBMS로 PostgreSQL을 사용하고있다. 관계형 데이터베이스 시스템들이 그렇게 큰 차이를 보이는 건 아니지만 mysql을 다루고 있는 책들이 워낙 많기도하고 데이터베이스를 공부하는 김에 mysql도 한번 정리해 본다.

## 설치
맥북을 이용하고 있기 때문에 Homebrew를 이용하여 설치했다
~~~shell
brew update
brew install mysql
~~~

## 서버 실행
서버를 실행해 주어야 한다. mysql이 설치된 곳으로 가서 아래와 같은 명령어를 입력한다
~~~shell
# /usr/local/bin
mysql.server start
~~~

## 서버 접속
brew를 통해 설치를 해 주었고 which 명령어를 통해 설치된 곳을 찾았다. 처음에는 기본 로그인 계정에 해당하는 아이디로 자동로그인을 시도하는데 비밀번호를 설정하지 않았기 때문에 실패하게 된다. 따라서 일단 실습을 진행하기 위해 root 계정으로 접속해보자.
~~~shell
mysql -u root
~~~

## 실습 준비
MySQL을 설치하면 기본적으로 다양한 샘플 데이터가 있다고한다. 하지만 brew를 통해서 설치해서 그런지 몰라도 아무런 샘플 데이터가 없었다. 실습을 준비하기 위해서 world 샘플 데이터를 받는 것부터 시작해보자. <https://dev.mysql.com/doc/index-other.html>에서 world 데이터를 받을 수 있다. 압축을 해제해고 적당한 곳에 저장한 후 데이터베이스에 목록에 추가해보자.
~~~shell
SOURCE /world.sql;
~~~
그리고 등록된 데이터 베이스를 살펴보자.
~~~shell
show databases;
~~~
~~~shell
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| world              |
+--------------------+
5 rows in set (0.00 sec)
~~~
world 데이터베이스가 추가된 것을 확인할 수 있다. 

## 등록된 데이터베이스 확인
~~~shell
# 복수형 databases임에 조심하자!!
show databases;
~~~

## 데이터베이스 선택과 테이블 확인
world 데이터베이스를 선택하고 테이블의 목록을 확인해보자.
~~~mysql
mysql> use world;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-----------------+
| Tables_in_world |
+-----------------+
| city            |
| country         |
| countrylanguage |
+-----------------+
3 rows in set (0.00 sec)
~~~

## 테이블을 지정해서 데이터 출력
city / country / countrylanguage 세 가지 테이블 중에서 하나를 골라 모든 데이터를 출력해보자
~~~shell
mysql> SELECT * FROM city;
+------+------------------------------------+-------------+------------------------+------------+
/////////////////////////////////////////////////////////////////////////////////////////////////
| 4072 | Mutare                             | ZWE         | Manicaland             |     131367 |
| 4073 | Gweru                              | ZWE         | Midlands               |     128037 |
| 4074 | Gaza                               | PSE         | Gaza                   |     353632 |
| 4075 | Khan Yunis                         | PSE         | Khan Yunis             |     123175 |
| 4076 | Hebron                             | PSE         | Hebron                 |     119401 |
| 4077 | Jabaliya                           | PSE         | North Gaza             |     113901 |
| 4078 | Nablus                             | PSE         | Nablus                 |     100231 |
| 4079 | Rafah                              | PSE         | Rafah                  |      92020 |
+------+------------------------------------+-------------+------------------------+------------+
4079 rows in set (0.01 sec)
~~~

## 조건 지정출력1
WHERE 구문을 이용해 지정된 조건에만 충족하는 데이터들을 추려낼 수 있다. 아래의 예시코드는 한국의 도시 이름만 추려서 출력하고있다. 
~~~shell
mysql> select * from city where countrycode="KOR";
+------+------------+-------------+---------------+------------+
| 2388 | Sangju     | KOR         | Kyongsangbuk  |     124116 |
| 2389 | Poryong    | KOR         | Chungchongnam |     122604 |
| 2390 | Kwang-yang | KOR         | Chollanam     |     122052 |
| 2391 | Miryang    | KOR         | Kyongsangnam  |     121501 |
| 2392 | Hanam      | KOR         | Kyonggi       |     115812 |
| 2393 | Kimje      | KOR         | Chollabuk     |     115427 |
| 2394 | Yongchon   | KOR         | Kyongsangbuk  |     113511 |
| 2395 | Sachon     | KOR         | Kyongsangnam  |     113494 |
| 2396 | Uiwang     | KOR         | Kyonggi       |     108788 |
| 2397 | Naju       | KOR         | Chollanam     |     107831 |
| 2398 | Namwon     | KOR         | Chollabuk     |     103544 |
| 2399 | Tonghae    | KOR         | Kang-won      |      95472 |
| 2400 | Mun-gyong  | KOR         | Kyongsangbuk  |      92239 |
+------+------------+-------------+---------------+------------+
70 rows in set (0.01 sec)
~~~

## 조건 지정출력2
이번에는 전라남도에 속한 도시만을 추려보자!
~~~shell
mysql> select * from city where district="chollanam";
+------+------------+-------------+-----------+------------+
| ID   | Name       | CountryCode | District  | Population |
+------+------------+-------------+-----------+------------+
| 2360 | Sunchon    | KOR         | Chollanam |     249263 |
| 2361 | Mokpo      | KOR         | Chollanam |     247452 |
| 2370 | Yosu       | KOR         | Chollanam |     183596 |
| 2390 | Kwang-yang | KOR         | Chollanam |     122052 |
| 2397 | Naju       | KOR         | Chollanam |     107831 |
+------+------------+-------------+-----------+------------+
5 rows in set (0.01 sec)
~~~

## 불필요한 열 제거하기
지금까지는 테이블에 있는 모든 열을 출력했지만 원하는 열만 보기위해 지정하여 출력할 수 있다. 다음의 예시는 Name과 Population 열만 출력하고 있다.
~~~shell
mysql> select Name, Population from city where district="chollanam";
+------------+------------+
| Name       | Population |
+------------+------------+
| Sunchon    |     249263 |
| Mokpo      |     247452 |
| Yosu       |     183596 |
| Kwang-yang |     122052 |
| Naju       |     107831 |
+------------+------------+
5 rows in set (0.01 sec)
~~~

## 다양한 조건 추가1
조건을 지정해 줄때 여러개도 가능하다. 전라남도에 속하면서 인구 수가 150000명이 넘는 도시를 출력했다.
~~~shell
mysql> select Name, Population from city where district="chollanam" and Population > 150000;
+---------+------------+
| Name    | Population |
+---------+------------+
| Sunchon |     249263 |
| Mokpo   |     247452 |
| Yosu    |     183596 |
+---------+------------+
3 rows in set (0.00 sec)
~~~

# 정리
테이블을 보기 위해선 데이터베이스를 선택해야 한다. 1개의 데이터베이스에는 복수의 테이블이 저장되어 있기 때문이다. 
데이터베이스의 목록을 보려면 **show databases;** 
데이터베이스를 지정하려면 **use <db_name>;** 
해당 데이터베이스의 테이블 목록을 보려면 **show tables;** 
명령을 사용하면 된다.
> 주의
> show 명령은 SQL의 명령어가 아니라 MySQL에서 제공하는 관리 명령이다

### SELECT 문
**SELECT 열 FROM 테이블명;** 
select 뒤에 column명을 from 뒤에 table명을 적어주면 된다. **select \***처럼 column에 *(별표)를 적어주게 되면 테이블의 전체 열을 지정할 수 있고, 임의의 열을 콤마(,)로 구분해 복수로 지정하는 것도 가능하다. 또한 FROM의 테이블 명도 **데이터베이스.테이블**과 같이 명시적으로 지정하는 것도 가능하다.

### WHERE 조건
원하는 column의 조건만을 충족하는 데이터들(row)만을 추리기 위해서는 **WHERE**를 이용해 조건을 지정해주면 된다. 
**SELECT 열명 FROM 테이블명 WHERE 조건;**


## 검색결과 정렬
한국에 있는 도시들을 인구가 적은 순으로 나열(오름차순) 해보자. - 검색 결과의 정렬에는 **ORDER BY**를 사용한다. 
~~~shell
mysql> select * from city where countrycode="KOR" order by population;
+------+------------+-------------+---------------+------------+
| ID   | Name       | CountryCode | District      | Population |
+------+------------+-------------+---------------+------------+
| 2400 | Mun-gyong  | KOR         | Kyongsangbuk  |      92239 |
| 2399 | Tonghae    | KOR         | Kang-won      |      95472 |
| 2398 | Namwon     | KOR         | Chollabuk     |     103544 |
| 2397 | Naju       | KOR         | Chollanam     |     107831 |
| 2396 | Uiwang     | KOR         | Kyonggi       |     108788 |
| 2395 | Sachon     | KOR         | Kyongsangnam  |     113494 |
| 2394 | Yongchon   | KOR         | Kyongsangbuk  |     113511 |
| 2393 | Kimje      | KOR         | Chollabuk     |     115427 |
| 2392 | Hanam      | KOR         | Kyonggi       |     115812 |
| 2391 | Miryang    | KOR         | Kyongsangnam  |     121501 |
| 2390 | Kwang-yang | KOR         | Chollanam     |     122052 |
+------+------------+-------------+---------------+------------+
~~~

## 테이블 집약
행수를 카운트하려면 아래와 같이 한다. 
~~~shell
mysql> select count(*) from city where countrycode="KOR";
+----------+
| count(*) |
+----------+
|       70 |
+----------+
1 row in set (0.01 sec)
~~~
카운트 뿐만 아니라 최소(min) / 최대(max) / 합계(sum) / 평균(avg)을 구할 수 있다. 
~~~shell
# 최소인구 / 최대인구 / 총 인구 / 평균 인구
mysql> select min(population), max(population), sum(population), avg(population) from city where countrycode="KOR";
+-----------------+-----------------+-----------------+-----------------+
| min(population) | max(population) | sum(population) | avg(population) |
+-----------------+-----------------+-----------------+-----------------+
|           92239 |         9981619 |        38999893 |     557141.3286 |
+-----------------+-----------------+-----------------+-----------------+
1 row in set (0.01 sec)
~~~





[참고]
<https://dev.mysql.com/doc/index-other.html>
<https://dev.mysql.com/doc/world-setup/en/world-setup-installation.html>
<https://unagi44.wordpress.com/2015/09/16/mac-os-x%EC%97%90%EC%84%9C-mysql-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%A4%ED%96%89/>
<http://aspdotnet.tistory.com/1848>
