---
title: CentOS의 yum 명령어 간단하게 알아보기
tags:
  - yum
  - linux
  - centos
categories:
  - linux
  - centos
date: 2019-03-06 16:37:00
---

## yum 명령어 정리

**레드햇 계열 리눅스인 CentOS는 패키지를 관리하기 위해 yum이라는 명령어를 사용한다.** yum은 Yellow-dog Updater Modified의 줄임말이며 RPM(Redhat Package Manager)을 통한 패키지 설치를 개선하기 위해 개발되었다.

yum에 대한 설정은 **/etc/yum.conf**에서 하고 있으며, **/etc/yum.repos.d/ 디렉토리**에 있는 파일에 지정된 서버주소로부터 패키지들을 설치하고 관리할 수 있다.

> * 설정파일 경로 : /etc/yum.conf
>
> * yum 레포지터리 파일 경로 : /etc/yum.repos.d/
>   * 위의 디렉토리에 있는 각 파일은 yum 명령어를 실행했을때 해당 패키지를 검색하는 네트워크 주소가 있다.

또한 RPM보다 편리하게 의존성 관리를 할 수 있다.



## 사용법

##### 기본 형태 

기본적인 형태는 아래와 같다

~~~shell
# 옵션 - 명령어 - 패키지명 순으로 적는다.
yum <option> <command> <package_name>
~~~

##### 패키지 설치

패키지를 설치하려면 다음과 같이하자.

~~~shell
# nginx 설치
yum install nginx
~~~

보통 패키지를 설치할때 필요한 용량과 계속해서 진행할 지 물어보는 과정이 있는데 **-y 옵션을 주게되면 끊김 없이 진행할 수 있다.**

~~~shell
yum -y install nginx
~~~

##### 업데이트

~~~shell
yum update nginx
~~~

##### 삭제

의존성관리를 개별적으로 해줄 필요없이 삭제할 수 있다.

~~~shell
# nginx 삭제
yum remove nginx
~~~

##### 서버에 있는 패키지 목록 확인

시스템에 설치된 패키지와 설치가능한 패키지의 목록을 출력한다.

~~~shell
yum list
~~~

##### 설치된 패키지 확인

현재 시스템에 설치되어있는 패키지 리스트를 출력한다. 

~~~shell
yum list installed
~~~

##### 설치가능한 패키지 확인

시스템에 설치 가능한 패키지를 출력한다. 보통  grep과 같이 사용하곤 한다.

~~~shell
yum list available | grep <name>
~~~

##### 업데이트가 필요한 패키지 확인

현재 시스템에 설치된 패키지들 중에서 업데이트가 필요한 패키지를 출력한다.

~~~shell
yum list updated
~~~

##### 패키지 정보 확인

~~~shell
yum info <package_name>
~~~

