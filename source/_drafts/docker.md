---
categories:
  - docker
title: docker
tags:
  - docker
  - container
  - virtualization
---
# docker 기본

도커는 컨테이너 기반의 오픈소스 가상화 플랫폼이다. 기존의 가상화 방식인 OS 가상화가 아닌 프로세스를 격리하는 방식이다. OS 가상화와 같은 전/반 가상화 방식은 추가적인 운영체제를 설치해야하기 때문에 성능상의 손실이 크게 발생한다. 그러나 도커의 경우 기존에 리눅스에 존재하던 cgroup과 lxc를 개량하여 프로세스를 격리하는 방식을 취하기 때문에 성능상의 손실이 전가상화 보다 적은 편이다.

위에서 언급한 장점과 또 다른 여러 장점들을 포함해 DevOps개념이 퍼지게 되면서 도커의 대중성이 커지게 되었다고 생각한다.

## 명령어
~~~shell
docker images
docker rmi <image_name>
~~~
현재 도커에 설치되어 있는 이미지들을 확인할 수 있다. 또한 **rmi 명령어를 통해 이미지를 삭제할 수 있다.**

~~~shell
docker search <iamge_name>
~~~
docker hub에 있는 이미지들을 검색한다.

~~~shell
docker ps
docker ps -a
~~~
현재 실행중인 이미지들을 출력한다. 옵션으로 **-a**을 주게되면 죽어있는 프로세스도 볼 수 있다.

~~~shell
docker run 
docker run -dit --name <alias_name>
~~~
도커 이미지를 실행한다. 첫 번째줄과 같이 실행을 하게 되면 실행과 동시에 프로세스가 죽게되며 **docker ps -a**명령어를 통해 확인할 수 있다. 옵션을 하나씩 살펴보면 **d**는 데몬으로 실행함을 의미하며 **i**는 인터렉티브 모드로 실행함을 **t**는 tty를 실행할 것임을 의미힌다. 또한 뒤의 **--name**는 alias 처럼 별칭을 주는 역할을 한다. 

~~~shell
docker attach <name>
exit
~~~
생성한 컨테이너 안으로 들어간다. <name>에 해당하는 부분에는 위에서 지어준 별칭을 통해서도 접근이 가능하다. 접속한 컨테이너로 부터 나오고 싶으면 **exit**를 입력하면 된다. 여기서 알고 넘어가야 할 것은 **attach를 통해 컨테이너에 접속후 exit를 입력해서 컨테이너를 빠져 나오게되면 프로세스가 자동으로 죽는 다는 것이다.** 

~~~shell
docker start <name>
docker exec -it <name> bash
docker exec -it <name> echo hello world # hello world 출력
~~~
보통 컨테이너에 접속하게 될 때, **attach 보다는 start를 이용하는 편이라고 한다.** 우선 start 명령어를 통해 도커 컨테이너를 실행시켜 놓고 **exec 명령어로 interactive / tty 옵션을 주어서 bash 쉘로 접속하는 방법이다. exec 명령어를 통해 컨테이너에 접속을 하게되면 exit로 나갈경우에도 컨테이너가 종료되지 않고 계속해서 남아있다.** exec 명령어는 단순히 말 그대로 적혀있는 명령어를 한 번 실행할 뿐이다. 보통 bash 쉘을 접속하기 위해 bash를 적는 편이지만 계속해서 실행할 필요없이 한 번의 커맨드만 필요로 한다면 exec를 사용하면 된다.

~~~shell
docker inspect <name>
~~~
해당 컨테이너의 설정(path, ip) 값들을 확인할 수 있다. 

~~~shell
docker commit <from_container> <to_image_name>
~~~
나만의 도커 이미지를 생성할 수 있다. 

~~~shell
docker start <container_name>
docker stop <container_name>
~~~
이미지로 생성된 도커의 컨테이너를 실행하고 중지한다. stop으로 중지시킨 컨테이너는 일반적인 ps 명령어를 통해 볼 수 없으며 **docker ps -a** 통해 출력할 수 있다. 

~~~shell
docker rm <container_name>
~~~
생성된 컨테이너를 삭제한다. 다만, 컨테이너가 실행되지 않은 상태에서만 일반적으로 사용 가능하며 강제로 지우는 방법은 따로 존재한다. 