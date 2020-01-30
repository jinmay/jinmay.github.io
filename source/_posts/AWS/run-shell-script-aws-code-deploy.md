---
title: '[CodeDeploy]AWS CodeDeploy 배포 후 bash script 실행하기'
categories:
  - aws
---

AWS의 codeDeploy로 서버에 코드 배포 후 shell script 실행하는 법을 정리한다.

_appspec.yml_ 파일로 codeDeploy의 설정을 할 수 있다. hooks 섹션에 적어주어야 하며 codeDeploy를 통해 app을 설치하기 전 / 후로 작업을 수행할 수 있다. hooks 섹션에는 여러 개의 실행 순서가 있는데 [여기](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html)를 참고하자.

주의해야 할 점으로는 실행할 쉘 스크립트는 EC2가 아닌 배포하려는 프로젝트 안에 존재해야 한다는 것이다.

## appspec.yml 예시

```yml
version: 0.0
os: linux
files:
  - source: /
    destination: /srv/app

hooks:
  AfterInstall:
    # EC2경로가 아니라 배포할 소스 코드에 shell script가 있어야 함에 유의!!
    - location: scripts/after_deploy.sh
      runas: root
```

---

[참고]  
https://suwoni-codelab.com/aws/2018/03/24/aws-after-script/
