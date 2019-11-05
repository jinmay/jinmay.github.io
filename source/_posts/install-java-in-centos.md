---
title: centos7에서 java8 버전 이상 설치하기
tags:
  - java
  - elasticsearch
  - elk
categories:
  - java
date: 2018-06-22 17:44:27
---

# centos7 에서 java8 버전 이상 설치하기
지금까지 도커를 통해 로컬에서 ELK 스택을 연습했다. 실제 알리미 서버에 적용해보기 위해서 시도하고 있는데 우분투가 아닌 centos7이라서 설치하는 방법들이 조금은 다른 것 같다. 그래서 처음부터 정리하려고 한다!


## java8 이상 설치하기
엘라스틱서치는 자바8 이상을 필요로한다. 그러므로 자바를 설치하자! 
~~~text
wget --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/10.0.1+10/fb4372174a714e6b8c52526dc134031e/jdk-10.0.1_linux-x64_bin.tar.gz
~~~
위 의 명령은 자바10을 설치한다. 만약 다른 버전의 자바를 설치하고 싶다면 오라클 자바 다운로드 페이지에서 원하는 버전의 파일의 경로를 획득한 뒤 주소만 바꿔서 사용하자.

설치 디렉토리는 자유지만 그냥 이렇게했다.
~~~sh
mkdir /usr/local/java
mv jdk-8u112-linux-x64.tar.gz /usr/local/java
tar xvzf jdk-8u112-linux-x64.tar.gz
~~~

명령어를 등록한다.
~~~sh
alternatives --install /usr/bin/java java /usr/local/java/jdk-10.0.1/bin/java 1
alternatives --install /usr/bin/java javac /usr/local/java/jdk-10.0.1/bin/javac 1
alternatives --install /usr/bin/java javaws /usr/local/java/jdk-10.0.1/bin/javaws 1

alternatives --set java /usr/local/java/jdk-10.0.1/bin/java
alternatives --set javac /usr/local/java/jdk-10.0.1/bin/javac
alternatives --set javaws /usr/local/java/jdk-10.0.1/bin/javaws
~~~
자바10을 다운로드 받았기 때문에 위와 같이 설정해주었다.

다음으로 환경변수를 설정해주어야한다.
~~~sh
# in ~/.zshrc

export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")
~~~
마찬가지로 각자 해당하는 쉘의 conf을 수정해야한다. (나의 경우 zsh을 사용중이기 때문에 .zshrc를 수정했다)

마지막으로 자바 설치가 제대로 됐는지 확인한다.
~~~sh
java -version
# java version "10.0.1" 2018-04-17
# Java(TM) SE Runtime Environment 18.3 (build 10.0.1+10)
# Java HotSpot(TM) 64-Bit Server VM 18.3 (build 10.0.1+10, mixed mode)
~~~