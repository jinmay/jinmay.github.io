---
title: how-to-install-code-deploy-agent-ubuntu
---

'[CodeDeploy]EC2 우분투 인스턴스에 CodeDeploy Agent 설치하기'

배포할 서버에 접속하여 최초 한 번만 설치해주면 된다.

### 1. apt 저장소 업데이트

```sh
sudo apt-get update
```

### 2. ruby 설치

CodeDeploy Agent는 ruby라는 프로그래밍 언어로 작성되어있다. 따라서 ruby를 설치해주어야 한다.

```sh
# ubuntu 16.04
sudo apt-get install ruby

# ubuntu 14.04
sudo apt-get install ruby2.0
```

### 3. wget 설치

```sh
sudo apt-get install wget
```

### 4. Agent 설치

wget으로 Agent의 설치파일을 다운받는 과정에 bucket-name과 region-identifier를 적절히 적어주어야 한다. [여기](https://docs.aws.amazon.com/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names)를 참고해서 원하를 region으로 설정하면 된다.

한국 리전을 사용하려면  
**wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install**  
로 설치하면 된다.

```sh
# install 파일 경로는 원하는 대로 가능
cd /home/ubuntu

# bucket-name과 region-identifier를 각자의 상황에 맞게 변경
# wget https://<bucket-name>.s3.<region-identifier>.amazonaws.com/latest/install

# Seoul region
wget https://aws-codedeploy-ap-northeast-2.s3.ap-northeast-2.amazonaws.com/latest/install

chmod +x ./install
sudo ./install auto
```

현재까지 에러가 발생하지 않았다면 문제 없이 Agent 설치에 성공한 것이다. 아래의 명령어는 Agent의 상태확인과 서비스를 실행시켜준다.

```sh
# codedeploy-agent 상태 확인
sudo service codedeploy-agent status

# codedeploy-agent 서비스 시작
sudo service codedeploy-agent start
```

---

[참고]  
https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent-operations-install-ubuntu.html
https://docs.aws.amazon.com/codedeploy/latest/userguide/resource-kit.html#resource-kit-bucket-names
