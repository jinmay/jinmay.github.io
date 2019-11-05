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
~~~sh
brew update
brew install mysql
~~~

## 서버 실행
서버를 실행해 주어야 한다. mysql이 설치된 곳으로 가서 아래와 같은 명령어를 입력한다
~~~sh
# /usr/local/bin
mysql.server start
~~~

## 서버 접속
brew를 통해 설치를 해 주었고 which 명령어를 통해 설치된 곳을 찾았다. 처음에는 기본 로그인 계정에 해당하는 아이디로 자동로그인을 시도하는데 비밀번호를 설정하지 않았기 때문에 실패하게 된다. 따라서 일단 실습을 진행하기 위해 root 계정으로 접속해보자.
~~~sh
mysql -u root
~~~

## 실습 준비
MySQL을 설치하면 기본적으로 다양한 샘플 데이터가 있다고한다. 하지만 brew를 통해서 설치해서 그런지 몰라도 아무런 샘플 데이터가 없었다. 실습을 준비하기 위해서 world 샘플 데이터를 받는 것부터 시작해보자. <https://dev.mysql.com/doc/index-other.html>에서 world 데이터를 받을 수 있다. 압축을 해제해고 적당한 곳에 저장한 후 데이터베이스에 목록에 추가해보자.
~~~sh
SOURCE /world.sql;
~~~
그리고 등록된 데이터 베이스를 살펴보자.
~~~sh
show databases;
~~~
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
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
~~~sh
mysql> select count(*) from city where countrycode="KOR";
+----------+
| count(*) |
+----------+
|       70 |
+----------+
1 row in set (0.01 sec)
~~~
카운트 뿐만 아니라 최소(min) / 최대(max) / 합계(sum) / 평균(avg)을 구할 수 있다. 
~~~sh
# 최소인구 / 최대인구 / 총 인구 / 평균 인구
mysql> select min(population), max(population), sum(population), avg(population) from city where countrycode="KOR";
+-----------------+-----------------+-----------------+-----------------+
| min(population) | max(population) | sum(population) | avg(population) |
+-----------------+-----------------+-----------------+-----------------+
|           92239 |         9981619 |        38999893 |     557141.3286 |
+-----------------+-----------------+-----------------+-----------------+
1 row in set (0.01 sec)
~~~
count처럼 행의 갯수를 세는 것과 sum / min 처럼 column의 집약도 가능하지만 **문자열 집약**도 할 수 있다.
~~~sh
mysql> select name from city where countrycode="KOR" and district="Chollabuk";
+----------+
| name     |
+----------+
| Chonju   |
| Iksan    |
| Kunsan   |
| Chong-up |
| Kimje    |
| Namwon   |
+----------+
6 rows in set (0.00 sec)
~~~
한 행으로 집약하려면 아래와 같이 한다.
~~~sh
mysql> select group_concat(name) from city where countrycode="KOR" and district="Chollabuk";
+-------------------------------------------+
| group_concat(name)                        |
+-------------------------------------------+
| Chonju,Iksan,Kunsan,Chong-up,Kimje,Namwon |
+-------------------------------------------+
1 row in set (0.00 sec)
~~~

## group by
그룹별로 나눌 수도 있다. - group by 이용
~~~sh
ysql> select district, count(*) from city where countrycode="KOR" group by district;
+---------------+----------+
| district      | count(*) |
+---------------+----------+
| Cheju         |        1 |
| Chollabuk     |        6 |
| Chollanam     |        5 |
| Chungchongbuk |        3 |
| Chungchongnam |        6 |
| Inchon        |        1 |
| Kang-won      |        4 |
| Kwangju       |        1 |
| Kyonggi       |       18 |
| Kyongsangbuk  |       10 |
| Kyongsangnam  |       11 |
| Pusan         |        1 |
| Seoul         |        1 |
| Taegu         |        1 |
| Taejon        |        1 |
+---------------+----------+
15 rows in set (0.00 sec)
~~~
그룹지어서 결과를 보고자 할때 having 키워드를 이용해서 조건을 지정해 줄 수 있다
~~~sh
mysql> select district, count(*) from city where countrycode="KOR" group by district having count(*) = 1;
+----------+----------+
| district | count(*) |
+----------+----------+
| Cheju    |        1 |
| Inchon   |        1 |
| Kwangju  |        1 |
| Pusan    |        1 |
| Seoul    |        1 |
| Taegu    |        1 |
| Taejon   |        1 |
+----------+----------+
7 rows in set (0.00 sec)
~~~

# 정리
## 검색결과 정렬
보통 SELECT 문을 실행하면 열과 행으로 구성된 2차원 표로 표시된다. 행의 순서를 정렬하기 위해 **ORDER BY**를 사용해서 할 수 있으며 기본 값은 오름차순이고 값을 **DESC**로 줌으로써 내림차순 정렬도 할 수 있다.
~~~sh
mysql> select * from city where countrycode="KOR" order by population desc;
+------+------------+-------------+---------------+------------+
| ID   | Name       | CountryCode | District      | Population |
+------+------------+-------------+---------------+------------+
| 2331 | Seoul      | KOR         | Seoul         |    9981619 |
| 2332 | Pusan      | KOR         | Pusan         |    3804522 |
| 2333 | Inchon     | KOR         | Inchon        |    2559424 |
| 2334 | Taegu      | KOR         | Taegu         |    2548568 |
| 2335 | Taejon     | KOR         | Taejon        |    1425835 |
| 2336 | Kwangju    | KOR         | Kwangju       |    1368341 |
| 2337 | Ulsan      | KOR         | Kyongsangnam  |    1084891 |
| 2338 | Songnam    | KOR         | Kyonggi       |     869094 |
| 2339 | Puchon     | KOR         | Kyonggi       |     779412 |
| 2340 | Suwon      | KOR         | Kyonggi       |     755550 |
| 2341 | Anyang     | KOR         | Kyonggi       |     591106 |
| 2342 | Chonju     | KOR         | Chollabuk     |     563153 |
~~~

## 정렬할 때에 주의할 점
order by로 정렬을 수행할 때 행의 순서를 확실히 같게 하려면 행의 정렬키의 값들이 각각 고유해야 한다. 다시 말하면, 정렬키가 같은 값의 행이 복수 개 존재한다면 그 행들의 순서는 일정하지 않을 수 있다는 것이다. 
~~~sh
mysql> select * from city where countrycode="KOR" order by district;
+------+------------+-------------+---------------+------------+
| ID   | Name       | CountryCode | District      | Population |
+------+------------+-------------+---------------+------------+
| 2358 | Cheju      | KOR         | Cheju         |     258511 |
| 2398 | Namwon     | KOR         | Chollabuk     |     103544 |
| 2380 | Chong-up   | KOR         | Chollabuk     |     139111 |
| 2342 | Chonju     | KOR         | Chollabuk     |     563153 |
| 2397 | Naju       | KOR         | Chollanam     |     107831 |
| 2370 | Yosu       | KOR         | Chollanam     |     183596 |
| 2390 | Kwang-yang | KOR         | Chollanam     |     122052 |
| 2361 | Mokpo      | KOR         | Chollanam     |     247452 |
| 2360 | Sunchon    | KOR         | Chollanam     |     249263 |
~~~
위의 결과를 보면 district를 정렬키로 정렬했는데 전라남도는 5개의 행이 있는데 그 다섯 개 행 내의 순서는 일정하지 않게 된다.
일정한 순서로 정렬하기 위해서 name도 정렬키로 추가해주는 것이 좋다.
~~~sh
mysql> select * from city where countrycode="KOR" order by district, name;
~~~

## 테이블 요약
SQL에서 데이터에 대해 어떠한 ㅗ짝이나 계산을 수행하려면 **함수**를 사용한다. 함수는 크게 두 가지가 있는데
1. 복수 행(or 행의 값)에 대해 집계
2. 단일 행의 값에 대해 조작이나 계산
을 수행하는 역할을 한다.

예를 들면 위에서 한번 살펴본 **count 함수**이다. 이런 집계용 함수를 **집약함수(집계함수)**라고 부른다. 
* COUNT : 테이블의 행 수를 계산
* SUM : 테이블의 수치 데이터를 합계
* AVG : 테이블의 수치 데이터 평균을 구함
* MAX : 테이블의 임의열 데이터 중 최대값을 구함
* MIN : 테이블의 임의열 데이터 중 최소값을 구함

기본적으로 집계함수는 NULL을 제외하고 계산한다. 하지만 **COUNT함수는 예외인데 NULL을 포함한 조건으로 주어진 모든 행을 집계한다.**

## 문자열 집약
문자열을 집약하는 함수로는 **GROUP_CONCAT**가 있다. 표준 SQL에는 없지만 MySQL에서 제공해주는 함수이다. SUM과 같은 함수는 수치에 대해서 집계를 해주지만 GROUP_CONCAT 함수는 문자열에 대한 집계를 문자열의 결합으로 수행한다. 

## 중복제거
**DISTINCT** 키워드를 통해 중복을 제거할 수 있다. 또한 집약함수로도 이용할 수 있다.
~~~sh
mysql> select group_concat(district) from city where countrycode="KOR";
| Seoul,Pusan,Inchon,Taegu,Taejon,Kwangju,Kyongsangnam,Kyonggi,Kyonggi,Kyonggi,Kyonggi,Chollabuk,Chungchongbuk,Kyonggi,Kyonggi,Kyongsangbuk,Kyongsangnam,Kyongsangnam,Kyonggi,Chungchongnam,Kyongsangnam,Chollabuk,Kyonggi,Kyongsangbuk,Kyonggi,Kyongsangbuk,Chollabuk,Cheju,Kyongsangnam,Chollanam,Chollanam,Kyonggi,Kang-won,Kyonggi,Kang-won,Kyonggi,Kang-won,Chungchongbuk,Kyongsangbuk,Chollanam,Kyongsangbuk,Kyonggi,Kyongsangnam,Kyonggi,Chungchongnam,Kyongsangnam,Kyongsangbuk,Chungchongnam,Kyonggi,Chollabuk,Chungchongbuk,Chungchongnam,Kyonggi,Kyongsangnam,Chungchongnam,Kyongsangbuk,Kyongsangnam,Kyongsangbuk,Chungchongnam,Chollanam,Kyongsangnam,Kyonggi,Chollabuk,Kyongsangbuk,Kyongsangnam,Kyonggi,Chollanam,Chollabuk,Kang-won,Kyongsangbuk |
~~~
위 와 같이 단순히 행정구역을 group_concat하게 되면 값이 여러번 나오게된다. 이때에 distinct를 추가하게 되면 중복이 없어지고 각 행정구역은 1회만 나오게 된다.
~~~sh
mysql> select group_concat(distinct district) from city where countrycode="KOR";
+------------------------------------------------------------------------------------------------------------------------------------------+
| group_concat(distinct district)                                                                                                          |
+------------------------------------------------------------------------------------------------------------------------------------------+
| Seoul,Pusan,Inchon,Taegu,Taejon,Kwangju,Kyongsangnam,Kyonggi,Chollabuk,Chungchongbuk,Kyongsangbuk,Chungchongnam,Cheju,Chollanam,Kang-won |
+------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
~~~

## 그룹핑(group_by)
지금까지 한 집약함수와는 다르게 대상이 되는 데이터를 몇 개의 그룹으로 나누어서 집약하는 것도 가능하다. **GROUP_BY**를 이용하여 그룹을 나눌 때는 키가 되는 열을 지정해 주어야 한다. 
~~~sh
mysql> select count(*) from city where countrycode="KOR";
+----------+
| count(*) |
+----------+
|       70 |
+----------+
1 row in set (0.00 sec)


mysql> select district, count(*) from city where countrycode="KOR" group by district;
+---------------+----------+
| district      | count(*) |
+---------------+----------+
| Cheju         |        1 |
| Chollabuk     |        6 |
| Chollanam     |        5 |
| Chungchongbuk |        3 |
| Chungchongnam |        6 |
| Inchon        |        1 |
| Kang-won      |        4 |
| Kwangju       |        1 |
| Kyonggi       |       18 |
| Kyongsangbuk  |       10 |
| Kyongsangnam  |       11 |
| Pusan         |        1 |
| Seoul         |        1 |
| Taegu         |        1 |
| Taejon        |        1 |
+---------------+----------+
15 rows in set (0.00 sec)
~~~

그룹으로 나눈 후에 집약하기 위해서 조건을 추가해 줄 수 있다. 이전에 조건을 추가하기 위해서 where 구문을 사용한 적이 있는데 **count 같은 집약 함수를 작성할 수 있는 경우는 SELECT와 ORDER BY 그리고 HAVING 밖에 없다.** 따라서 집약한 결과에 조건을 지정하기 위해서는 **HAVING** 구문을 사용해야만 한다. 
~~~sh
mysql> select district, count(*) from city where countrycode="KOR" group by district having count(*) = 6;
+---------------+----------+
| district      | count(*) |
+---------------+----------+
| Chollabuk     |        6 |
| Chungchongnam |        6 |
+---------------+----------+
2 rows in set (0.00 sec)
~~~





[참고]
<https://dev.mysql.com/doc/index-other.html>
<https://dev.mysql.com/doc/world-setup/en/world-setup-installation.html>
<https://unagi44.wordpress.com/2015/09/16/mac-os-x%EC%97%90%EC%84%9C-mysql-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%8B%A4%ED%96%89/>
<http://aspdotnet.tistory.com/1848>
