---
title: '[CodeDeploy]CodeDeploy EC2/온프레미스 배포에 대한 로그 데이터 위치'
categories:
  - aws
---

> 참고: AWS Lambda 또는 Amazon ECS 배포에는 로그가 지원되지 않는다.

개별 인스턴스에 대한 **CodeDeploy 배포 로그 데이터를 보려면 아래의 경로를 참고**한다. Windows Server를 제외한 Amazon Linux, RHEL, Ubuntu Server 인스턴스에서 확인할 수 있다.

**경로: /opt/codedeploy-agent/deployment-root/deployment-logs/codedeploy-agent-deployments.log**

CodeDeploy가 설치되어 있는 인스턴스에서 CodeDeploy Agent의 로그를 확인하려면 아래의 경로를 확인한다.

**경로: /var/log/aws/codedeploy-agent**

참고로 CodeDeploy Agent가 설치되어 있는 경로는 /opt/codedeploy-agent 이다.

아래의 명령어를 통해 agent의 상태를 확인할 수 있다.

```sh
sudo service codedeploy-agent status
```

---

[참고]  
https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/deployments-view-logs.html#deployments-view-logs-instance-unix  
https://americanopeople.tistory.com/284
