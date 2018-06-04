---
title: 기초적인 Docker 사용법
tags:
  - docker
  - container
  - virtualization
categories:
  - docker
date: 2018-06-04 01:01:55
---

# docker 기본

도커는 컨테이너 기반의 오픈소스 가상화 플랫폼이다. 기존의 가상화 방식인 OS 가상화가 아닌 프로세스를 격리하는 방식이다. OS 가상화와 같은 전/반 가상화 방식은 추가적인 운영체제를 설치해야하기 때문에 성능상의 손실이 크게 발생한다. 그러나 도커의 경우 기존에 리눅스에 존재하던 cgroup과 lxc를 개량하여 프로세스를 격리하는 방식을 취하기 때문에 성능상의 손실이 전가상화 보다 적은 편이다.

위에서 언급한 장점과 또 다른 여러 장점들을 포함해 DevOps개념이 퍼지게 되면서 도커의 대중성이 커지게 되었다고 생각한다.

## 명령어

### 이미지 목록 / 삭제
~~~shell
docker images
docker rmi <image_name>
~~~
현재 도커에 설치되어 있는 이미지들을 확인할 수 있다. 또한 **rmi 명령어를 통해 이미지를 삭제할 수 있다.**

### 이미지 검색
~~~shell
docker search <iamge_name>
~~~
docker hub에 있는 이미지들을 검색한다.

### 컨테이너 생성
~~~shell
docker create -it --name <container_name> <image_name>
~~~
컨테이너를 생성하는 방법에는 생성과 동시에 실행하는 docker 명령어와 **컨테이너를 생성만 하는 docker create 명령어가 있다.** 단지 컨테이너를 만들기만 할뿐 실행을 하지는 않기 때문에 demon으로 돌리는 -d 옵션은 존재하지 않는다.

### 컨테이너 출력
~~~shell
docker ps
docker ps -a
~~~
현재 실행중인 이미지들을 출력한다. 옵션으로 **-a**을 주게되면 죽어있는 프로세스도 볼 수 있다.

### 컨테이너 실행
~~~shell
docker run 
docker run -dit --name <alias_name>
~~~
도커 이미지를 실행한다. 첫 번째줄과 같이 실행을 하게 되면 실행과 동시에 프로세스가 죽게되며 **docker ps -a**명령어를 통해 확인할 수 있다. 옵션을 하나씩 살펴보면 **d**는 데몬으로 실행함을 의미하며 **i**는 인터렉티브 모드로 실행함을 **t**는 tty를 실행할 것임을 의미힌다. 또한 뒤의 **--name**는 alias 처럼 별칭을 주는 역할을 한다. 

-d 옵션을 주면 백그라운드에서 데몬이 돌기 때문에 docker run -dit로 실행하게 되면 bash를 실행해도 바로 shell이 뜨지 않는다. **run**과 **create** 명령어의 차이를 알아보자.

1. create
docker pull(이미지가 없을때) > docker create
docker create 명령어는 도커 이미지를 pull한 뒤에 컨테이너를 생성만 할뿐 start / attach 를 실행하지 않는다. 보통 컨테이너를 생성함과 동시에 시작하기 때문에 run 명령어를 더 자주 사용한다.

2. run
docker pull(이미지가 없을때) > docker create > docker start > docker attach -it
docker run 명령어는 컨테이너를 생성과 동시에 실행하고 attach 하기 때문에(-it 옵션) 컨테이너 내부로 들어간다.


### 컨테이너 접속
~~~shell
docker attach <name>
exit
~~~
생성한 컨테이너 안으로 들어간다. <name>에 해당하는 부분에는 위에서 지어준 별칭을 통해서도 접근이 가능하다. 접속한 컨테이너로 부터 나오고 싶으면 **exit**를 입력하면 된다. 여기서 알고 넘어가야 할 것은 **attach를 통해 컨테이너에 접속후 exit를 입력해서 컨테이너를 빠져 나오게되면 프로세스가 자동으로 죽는 다는 것이다.** 그리고 docker ps를 통해 상태를 확인해보면 COMMAND 항목이 있는데 attach로 컨테이너에 들어갔을때 실행되는 환경이다. **예를 들면 nodejs를 pull 해올 경우 COMMAND가 /bin/bash가 아니라 node환경이기 때문에 docker attach 명령어를 통해 컨테이터에 접속할 경우 bash창이 아닌 js를 실행할 수 있는 nodejs 환경을 볼 수 있을 것이다.** 

### 컨테이너 접속(2)
~~~shell
docker start <name>
docker exec -it <name> bash
docker exec -it <name> echo hello world # hello world 출력
~~~
보통 컨테이너에 접속하게 될 때, **attach 보다는 start를 이용하는 편이라고 한다.** 우선 start 명령어를 통해 도커 컨테이너를 실행시켜 놓고 **exec 명령어로 interactive / tty 옵션을 주어서 bash 쉘로 접속하는 방법이다. exec 명령어를 통해 컨테이너에 접속을 하게되면 exit로 나갈경우에도 컨테이너가 종료되지 않고 계속해서 남아있다.** exec 명령어는 단순히 말 그대로 적혀있는 명령어를 한 번 실행할 뿐이다. 보통 bash 쉘을 접속하기 위해 bash를 적는 편이지만 계속해서 실행할 필요없이 한 번의 커맨드만 필요로 한다면 exec를 사용하면 된다.

### 컨테이너 검사
~~~shell
docker inspect <name>
~~~
해당 컨테이너의 설정(path, ip) 값들을 확인할 수 있다. 

### 이미지 생성
~~~shell
docker commit <from_container> <to_image_name>
~~~
나만의 도커 이미지를 생성할 수 있다. 

### 컨테이너 실행 / 중지
~~~shell
docker start <container_name>
docker stop <container_name>
~~~
이미지로 생성된 도커의 컨테이너를 실행하고 중지한다. stop으로 중지시킨 컨테이너는 일반적인 ps 명령어를 통해 볼 수 없으며 **docker ps -a** 통해 출력할 수 있다. 

### 컨테이너 삭제
~~~shell
docker rm <container_name>
~~~
생성된 컨테이너를 삭제한다. 다만, 컨테이너가 실행되지 않은 상태에서만 일반적으로 사용 가능하며 강제로 지우는 방법은 따로 존재한다. 


## Dockerfile
Dockerfile은 도커 이미지를 생성하기 위한 설정 파일이다. Dockerfile에 설정한대로 이미지를 편리하게 생성하는 데 도움을 준다.