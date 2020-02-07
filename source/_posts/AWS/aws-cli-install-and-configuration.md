---
title: '[AWS]AWS CLI Tool을 설치하고 기본적인 설정하기'
categories:
  - aws
---

AWS CLI는 커맨드라인 인터페이스 툴로써 cli를 기반으로 AWS의 리소스를 관리할 수 있게 해준다. aws cli를 통해 여러 개의 aws 제품을 사용할 수 있고 스크립트를 통해 자동화할 수도 있다.

### 설치

기본적으로 AWS CLI는 파이썬으로 만들어져있다. 맥에서는 brew를 통해 편리하게 설치할 수 있다.

```sh
brew update
brew install awscli # 설치

# 업데이트
brew upgrade awscli

# 설치 확인
awscli --v
```

우분투 리눅스 배포판을 사용중이라면 먼저 파이썬을 설치하고 파이썬의 패키지 매니저인 pip3를 이용하여 설치한다. 개인적으로 pyenv를 사용하는데 awscli의 경우 특정한 가상환경에서 실행되는 것보다 전역적으로 설치되는 것이 낫다고 생각하여 글로벌하게 설치하고 사용한다.

```sh
pip3 install awscli

# 설치 확인
awscli --v
```

### 기본 설정

AWS에 접속하여 관리콘솔을 사용하듯이 awscli에서도 인증 정보를 추가해야 한다. AWS IAM에서 사용자를 생성하여 액세스 키와 시크릿 키를 발급받자. 관리 콘솔을 통해 IAM에 접속하면 아래와 같은 메뉴를 볼 수 있다.

![image](https://user-images.githubusercontent.com/13075035/73726200-50765e80-4772-11ea-8eb7-c0f780fbc393.png)

사용자 추가 버튼을 누르면 아래와 같은 화면을 볼 수 있는데 사용자 이름을 적고 *AWS 엑세스 유형*은 *프로그래밍 방식 엑세스*를 선택하도록 한다.

![image](https://user-images.githubusercontent.com/13075035/73726270-81ef2a00-4772-11ea-8819-680f7ff28340.png)

적절한 권한을 설정하고 사용자 추가를 마무리하면 액세스 키와 시크릿 키를 아래와 같이 받을 수 있다. 엑세스 키는 외부에 노출되어도 되지만 시크릿 키는 안되며 사용자 생성시 한 번만 볼 수 있기 때문에 다른데에 적어 두거나 csv 파일로 받아 두는 것이 좋다. **시크릿키 관리에 유의하자!!!**

![image](https://user-images.githubusercontent.com/13075035/73726394-ce3a6a00-4772-11ea-98f1-4a681b02091b.png)

엑세스 키와 시크릿 키를 발급 받았다면 awscli를 사용하려는 환경에 등록해야 한다. 엑세스 키를 등록하는 방법은 두 가지로 첫 번째, configure 명령어를 사용하는 것과 두 번째, 직접 파일을 수정하는 방법이 있다.

#### 1. configure 명령어

configure 명령어를 사용하면 커맨드라인에서 인터렉티브하게 설정을 할 수 있다.

```sh
aws configure

# 아래의 네 가지를 설정할 수 있다.
AWS Access Key ID # 엑세스 키
AWS Secret Access Key # 시크릿 키
Default region name # 기본 리전
Default output format # 기본 출력 포멧
```

엑세스 키와 시크릿 키는 IAM을 통해 받은 키 값을 넣으면 된다. region의 경우 사용하고픈 리전을 선택하면 되는데 서울 리전은 **ap-northeast-2** 을 이용하면 된다.

#### 2. 파일 직접 수정

awscli와 관련된 정보들은 두 가지 파일에 나뉘어서 저장된다.

- ~/.aws/credentials

credentials 파일에는 인증과 관련된 정보들이 저장되어있다. 대표적으로 aws_secret_access_key와 aws_access_key_id이 저장되어 있을 것이다.

- ~/.aws/config

인증 관련 정보 이외의 모든 설정이 저장된다.

### 정리

직접 파일을 만들어서 설정해도 무방하지만 configure 명령어를 사용하면 좀 더 편리하게 할 수 있을 것 같다. 여러 개의 IAM 사용자 계정을 한 번에 사용할 수도 있는데, 이는 추후에 정리할 계획이다.

---

[참고]  
https://aws.amazon.com/ko/cli/  
https://www.44bits.io/ko/post/aws_command_line_interface_basic#%EB%A7%A5osmacos
