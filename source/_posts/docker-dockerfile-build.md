---
title: Dockerfile을 통해서 나만의 이미지 만들기
categories:
  - docker
date: 2019-10-17 15:22:03
tags:
---

# Dockerfile을 통해서 나만의 이미지 만들기

**Dockerfile이라는 이름으로 파일을 만들고 빌드하면 나만의 이미지를 생성할 수 있다.** Dockerfile을 만들때 자주 사용되는 옵션들과 빌드할때의 옵션들을 정리해본다.

## Dockerfile

별다른 확장자없이 이름을 Dockerfile이라고 짓고 파일을 만들어준다. 원하는 편집기를 통해 작성하면된다. python3 이미지를 기반으로 작성할 것이며 간단한 Django 프로젝트를 실행하는 이미지를 만들것이다.

### From 

~~~Dockerfile
FROM 이미지:태그
FROM python:3
~~~

베이스 이미지를 지정한다. 반드시 베이스 이미지를 지정해야하고 태그는 필수적이지 않다. 하지만 본인이 사용할 버전을 명시적으로 적어두는 걸 추천한다.

~~~Dockerfile
RUN apt-get update && apt-get -y install libpq-dev
~~~

명령어를 실행한다. 내부적으로 /bin/sh -c 뒤에 명령어를 실행한다. 

~~~Dockerfile
RUN ["/bin/bash", "-c", "echo hello"]
~~~

만약 sh쉘이 아닌 다른 쉘을 사용하고 싶다면 직접 변경해주어야 한다.

### CMD

~~~Dockerfile
CMD ["python", "manage.py", "runserver", "0:8000"]

# 형태
CMD ["executable","param1","param2"]
CMD ["param1","param2"]
CMD command param1 param2
~~~

도커 컨테이너가 실행되었을 때 실행할 명령어이다. 주의해야 할 점으로 이미지를 빌드할때 실행하는 것이 아니라 **빌드된 이미지로 부터 생성된 컨테이너가 실행될때 마지막으로 실행한다는 것**이다. 그리고 **만약에 한 개 이상의 CMD 명령어가 있다면 가장 마지막의 CMD가 실행된다.**

### EXPOSE

~~~Dockerfile
EXPOSE 8000
EXPOSE 80 443
~~~

도커 컨테이너가 사용할 포트를 지정한다. 여러개를 사용할 수 있으며 띄어쓰기로 나누어 표현하면 된다.

### ENV

~~~Dockerfile
ENV <key> <value>
ENV <key>=<value>
~~~

컨테이너 내부에서 사용할 환경변수를 지정한다. 주의할 점으로는 컨테이너를 생성할때 -e 옵션을 지정해 준다면 기존값을 오버라이딩 할 수 있다는 것이다. 

### ADD

~~~Dockerfile
ADD ./requirements.txt /app/
ADD . /app
ADD ./manage.py /app/

ADD <src> <container_src>
~~~

파일 또는 폴더를 컨테이너 안으로 추가한다. **로컬의 src를 컨테이너의 src로 복사한다고 생각하면 될 것 같다.** 파일 또는 폴더만 가능한 것이 아니라 URL도 입력 가능하다.

### COPY

~~~Dockerfile
COPY <src>... <container_src>
COPY . /app
~~~

ADD와 매우 비슷하다. 만약 명시한 container_src 폴더가 없다면 자동으로 생성한다.

### WORKDIR

~~~Dockerfile
WORKDIR /app

WORKDIR /path/to/workdir
~~~

명령어들이 실행될 디렉터리를 지정한다. 각 명령어가 실행될 디렉터리가 초기화 되기 때문에 사용한다. 즉, RUN / CMD / ADD와 같은 Dockerfile 명령어들은 매 실행때마다 실행 위치가 초기화되며, 계속해서 같은 디렉터리에서 작업하기 위해 WORKDIR을 사용한다고 보면 된다.

나중에 docker-compose 부분에서 한 번 더 정리할 거지만, **도커 명령어가 실행될 context라는 게 매우 중요한 것 같다.**

### VOLUME

~~~Dockerfile
VOLUME ["/data"]
~~~

다른 컨테이너 또는 호스트로부터 마운트 포인트를 만든다.



지금까지 Dockerfile에서 자주 사용되는 명령어들에 대해서 간단하게 정리해보았다. 더 많은 명령어들이 있지만 아직은 이정도 까지만 자주 사용하는 정도이며, 다른 내용이 필요하다면 아래의 참고를 확인하자.


## 빌드하기

Dockerfile을 만들었다면 실제로 이미지를 만들기 위해 빌드하는 과정이 필요하다.

~~~sh
docker build [OPTIONS] PATH | URL | -
docker build 옵션 경로

docker build . -t my-image
docker build . -f compose/django/Dockerfile-dev -t my-image-dev
~~~

-t : 이미지 이름 지정
-f : (현재 폴더의 Dockerfile을 사용하지 않는다면) Dockerfile의 경로를 지정

맨 처음에는 다른 폴더에 있는 Dockerfile을 빌드하기 위해 -f을 사용하고 .(현재 경로)을 지정해 주지 않았다. **.(현재 경로)을 명시해주는 이유를 Dockerfile의 경로로 이해하고 사용했기 때문인데 아마 이게 아니라 도커 명령어를 실행하는 context였던거 같다.**

개발 / 배포 환경에 따른 Dockerfile 만들고 build를 하는 과정에서 많은 어려움이 있었다. 기본적으로 build 명령어는 .(현재 경로)의 Dockerfile을 참고하는데 -f 옵션을 몰라서 많은 시행착오를 겪었다.


<hr>
[참고]

<https://docs.docker.com/engine/reference/builder/>
<https://docs.docker.com/engine/reference/commandline/build/>