---
title: 도커와 함께하는 엘라스틱서치
tags:
  - elk
  - logstash
  - elasticsearch
  - kibana
categories:
  - docker
  - elasticsearch
date: 2018-06-21 16:02:23
---

# 도커와 함께하는 엘라스틱서치
갑자기 visual하게 통계를 보고 싶어서 오픈소스를 알아보던 중 엘라스틱서치를 접하게 되었다. 그런데 엘라스틱서치에 대해서 문서를 보던중 ELK 스택에 대해서 보게 되었고 보통 이 세가지가 같이 사용된다고 한다. ELK가 의미하는 것들은 다음과 같다.

* E : ElasticSearch - 분산형 RESTful 검색 및 분석 엔진
* L : Logstash - 로그 수집도구
* K : Kibana - 시각화툴(Visualization)

설치하는 법과 간단한 사용법들을 천천히 정리해보자

## 엘라스틱서치 설치
ELK 스택은 JVM 위에서 돌아가기 때문에 JDK를 필요로 한다. 때문에 자바를 우선 설치해주어야 한다.
~~~shell
apt-get update -y
add-apt-repository -y ppa:webupd8team/java
apt-get -y install oracle-java8-installer
~~~

만약 도커 컨테이너를 통해서 우분투를 설치하고 add-apt-repository 를 사용하려고 하면 해당 명령어가 없다고 할 것이다. 그럴땐 다음의 명령어를 실행해주고 재설치를 한다.
~~~shell
apt-get install software-properties-common

# 다시 설치도전!
add-apt-repository -y ppa:webupd8team/java
apt-get update -y
apt-get -y install oracle-java8-installer
java -version # 설치된 자바 버전 확인
~~~

자바를 설치했다면 이젠 엘라스틱서치를 설치하면된다.
~~~shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.1.deb
dpkg -i elasticsearch-5.3.1.deb
systemctl enable elasticsearch.service # (자동 재시작)
~~~
또한 관련 파일들의 경로는 아래와 같다.
* 설치 경로 : /usr/share/elasticsearch
* 설정파일 경로 : /etc/elasticsearch
* 스크립트 파일 : /etc/init.d/elasticsearch

앞으로 **service elasticsearch start** 명령어를 통해서 엘라스틱서치를 실행할 수 있다.
~~~shell
service elasticsearch start
service elasticsearch stop
service elasticsearch restart
~~~

설치후 정상적으로 작동한다면 curl 통해 제대로 동작하는지 본다
~~~shell
curl -XGET "localhost:9200"
~~~
~~~shell
# 출력 화면

{
  "name" : "StB5wZb",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "BG-SG1nxS0y8TlGtF7ffsdafsdubow",
  "version" : {
    "number" : "5.3.1",
    "build_hash" : "5f9cf58",
    "build_date" : "2017-04-17T15:52:53.846Z",
    "build_snapshot" : false,
    "lucene_version" : "6.4.2"
  },
  "tagline" : "You Know, for Search"
}
~~~
만약 엘라스틱서치의 설정 내용들은 보고 싶다면 **/etc/elasticsearch/elasticsearch.yml** 를 참고하면 된다. 나의 경우 건드려본 설정은 network.host 밖에 없다.


## 키바나 설치
다음으로 시각화 툴인 **키바나**를 설치해보자. 
~~~shell
wget https://artifacts.elastic.co/downloads/kibana/kibana-5.3.1-amd64.deb
dpkg -i kibana-5.3.1-amd64.deb
~~~

엘라스틱서치와 같이 사용하기 위해 **elasticsearch.yml**의 설정을 조금 건드려주어야 한다. 키바나 설정파일 경로는 **/etc/kibana/kibana.yml** 이다.
~~~shell
# vi /etc/kibana/kibana.yml
server.host: "localhost"
elasticsearch.url: "http://localhost:9200"
~~~
위의 경우에는 로컬에서 실행하기 때문에 localhost로 해주었지만 서버를 사용하고 있다면 해당 서버의 주소를 설정해주어야 한다. 

마지막으로 키바나를 실행해보자.
~~~shell
/usr/share/kibana/bin/kibana
~~~