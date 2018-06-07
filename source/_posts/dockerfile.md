---
title: dockerfile의 기본
tags:
  - dockerfile
categories:
  - docker
date: 2018-06-07 09:31:44
---

# Dockerfile
Dockerfile은 도커 이미지를 생성할때 실행할 일련의 명령어들을 적어놓은 파일이다. 쉽게 말하자면 Docker 이미지 설정 파일이다. 여러가지 명령을 토대로 기본 이미지에서 개발환경을 셋팅한 이미지를 생성하는 데에 큰 도움을 받을 수 있을 것 같다. 

보통 이렇게들 많이 사용하는 거 같다.
~~~shell
FROM  ubuntu

RUN apt-get update -y && apt-get install -y \
    vim
    curl
    git

CMD ["/bin/bash"]
~~~
간략하게 설명하자면 **어떠한 기본 이미지로부터 생성할 것인지를 FROM을 통해 적어준다. 그리고 어떠한 명령어를 실행할 지 RUN에 명시한다.**

그렇다면 자주 사용되는 Dockerfile의 커맨드에 대해서 알아보자.

* FROM 
사용할 기본 이미지를 적어준다. ubuntu:tags 처럼 태그가 표기된 버전도 사용 가능하다. 태그를 생력하면 latest를 사용한 것으로 간주하며 Dockerfile의 첫 번째 설정으로 명시되어야 한다.

* MAINTAINER
문자열을 "Author" 메타데이터로 설정한다. 이미지의 maintainer 이름, 연락처와 같은 성세 정보들을 설정하는 데 사용한다. 꼭 입력할 필요는 없지만 Docker hub등에 이미지 공개를 염두한다면 적어주는 것이 좋다.

* RUN
이미지 내부에서 수행될 쉘 커맨드를 작성한다. 

* ENV
이미지 내부의 환경 변수들을 설정한다. 

* USER
기본적으로 도커는 컨테이너 내에서 모든 프로세스를 root 계정으로 실행한다. 하지만 USER 라인을 통해서 바꿔줄 수 있다.

* ENTRYPOINT
컨테이너의 작동이 시작되었을때 (docker run / docker start) 지정된 스크립트 또는 명령을 실행한다. Dockerfile에서 딱 한번만 사용할 수 있다. 주어진 모든 인자들을 해석하기 전에 변수와 서비스들을 초기화하는 "시작 스크립트"를 제공하기 위해 사용하곤 한다. 

* CMD - 데몬 실행
컨테이너가 시작되는 시점에 실행할 스크립트 혹은 명령어를 작성한다. **만약 ENTRYPOINT가 Dockerfile에 정의되어 있다면 CMD는 Dockerfile의 ENTRYPOINT의 인자로 해석된다.** 또한 docker run의 이미지 이름 다음에 오는 인자의 의해서 재정의된다.(마지막 CMD 설정만이 유효하며 이전의 모든 CMD 설정들은 모두 재정의된다.)

* VOLUME
볼륨으로 마운트할 특정 파일이나 디렉토리를 정의한다. 컨테이너가 시작도리 때 볼륨으로 해당 파일이나 디렉터리가 복사된다. 만약 여러 개의 인자가 들어갔다면 여러 개의 불륨으로 인식한다. 

* WORKDIR
RUN / CMD / ENTRYPOINT / ADD / COPY 설정에서 사용할 디렉터리를 의미한다. 상대경로도 사용 가능하다. 


## Dockerfile을 통한 이미지 생성
~~~shell
docker build -t [new_image_name] [Dockerfile_path]
docker build -t test-dockerfile-image . # 해당 경로에 자동으로 Dockerfile을 찾아준다
~~~
