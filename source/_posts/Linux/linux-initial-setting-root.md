---
categories:
  - linux
  - ubuntu
date: 2019-02-22 17:48:30
title: 우분투 root 계정설정 가이드
tags:
  - linux
  - ubuntu
---

# 우분투 root 계정설정 가이드

리눅스 배포판을 설치하고나서 기본적으로 하면 좋은 설정들이 있다. 이러한 설정을 하는 이유에는 여러가지가 있겠지만 **보안적인 측면** 그리고 **쉬운 관리 및 더 나은 사용성**을 위해서 하는 세팅을 정리해보자.

> 개별 머신을 가지고 연습해보는 게 제일 좋겠지만 클라우드 플랫폼의 홍수 속에서 사는 만큼 그 이점을 활용하고자 AWS의 프리티어를 이용할 계획이다.

## 1. 루트계정(root) 비활성화

root / ec2-user 와 같은 유저는 모든 권한을 가지고 있는 계정이다. 일반적인 경우 root계정을 비활성화 시켜놓고 root권한을 가진 사용자계정을 새로 만들어 사용한다. AWS를 통해서 사용하고 있는 게 아니라면 root 권한을 가진 새로운 계정을 만든 후 root 계정을 비활성화 하고 AWS를 사용하고 있다면 ec2-user 또는 ubuntu 계정을 비활성화 하면 된다.

##### AWS을 사용한다면...

pem키를 통해서 ubuntu 계정으로 로그인하기 때문에 새로운 계정만 생성해서는 로그인을 할 수 없다. 아래와 같은 세 가지 과정을 거쳐야 한다.

> 1. 로그인에 사용할 새로운 계정 생성
> 2. pem키 재매핑(ssh-key 이전)
> 3. ubuntu 계정 삭제 또는 비활성화

##### 로그인 후 root  계정으로 전환

AWS의 경우 root 계정의 비밀번호가 설정되어 있지 않기 때문에 비밀번호 지정 후 이어나가야 한다.

```sh
sudo passwd root
```

##### 새로운 계정 생성

root의 패스워드를 지정했다면 root로 계전 전환 후 새로운 계정을 만든다.

```sh
adduser <newUser_id>
```

##### pem키 이전

ubuntu 계정으로 로그인 할 때 처럼 pem를 이용하기 위해서 이전을 시켜준다.

```sh
# 새로운 계정의 홈 폴더 아래에 .ssh 폴더 생성
mkdir /home/<newUser_id>/.ssh

# ubuntu 계정의 .ssh 폴더를 새로운 계정으로 복사
cp /home/ubuntu/.ssh /home/<newUser_id>/

# 소유자 변경
chown -R <newUser_id>:<newUser_id> /home/<newUser_id>/.ssh
```

##### 재시작

```sh
service sshd restart
```

지금부터 새로운 계정으로 pem키를 이용하여 로그인할 수 있을것이다. 하지만 sudo 명령어를 통해서 root 권한을 얻으려하면 에러가 발생하게된다. sudo 명령어를 사용할수 있게 만들기 위해서 sudoer 파일을 수정해주어야 한다.

> 아마도 _<username> is not in the sudoers file. This incident will be reported._ 이라는 에러가 발생할 것이다.

##### sudoers 파일 수정

sudoers 파일이란 sudo 명령어에 대한 설정을 하는 파일이며 sudo 명령어를 사용할 수 있는 계정을 지정할 수 있다. 파일의 위치는 /etc 폴더 아래에 있다(/etc/sudoers)

```sh
# vi /etc/sudoers

# User privilege specification
root    ALL=(ALL:ALL) ALL
example ALL=(ALL:ALL) ALL
```

root 아래에 새로운 계정명과 권한을 추가해 주면 된다

##### ubuntu 계정 삭제

계정과 홈 디렉토리를 삭제하기 위해 **-r 옵션**을 부여한다.

```sh
deluser -r ubuntu
```

##### 완료

지금까지 문제없이 잘 왔다면 기본유저 삭제 및 root 권한을 가질 수 있는 새로운 유저를 생성할 수 있다. 보안을 위해서 기본유저는 사용하지 말도록 하고 꼭 위와 같은 작업을 하는 것을 추천한다!

<hr>

<참고> 

https://blog.outsider.ne.kr/505

http://love0and0hate.blogspot.com/2017/01/linux-aws-ec2-user-ssh-key.html

https://uroa.tistory.com/100

https://blog.shako.net/ubuntu-server-16-04-initial-setup-guide/

