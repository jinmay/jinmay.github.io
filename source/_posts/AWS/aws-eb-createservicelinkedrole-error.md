---
title: CreateServiceLinkedRole 에러 해결하기
tags:
  - aws
  - elastic beanstalk
  - error
categories:
  - aws
  - elastic beanstalk
date: 2018-12-26 19:29:47
---

# eb-cli를 통해 deploy할 때 CreateServiceLinkedRole 에러 해결하기

AWS의 웹 콘솔을 벗어나 EB-CLI 환경에서 배포하던 도중에 **CreateServiceLinkedRole** 이라는 에러가 발생했다. 과정중에 잘못된게 있나 싶어서 테스트 프로젝트를 가지고 두 세번정도 시험삼아 해보았지만 에러는 계속해서 발생했다. 구글에서 검색을 통해 해결책을 찾던 중 IAM에서 인라인으로 정책을 부여하는 방법을 찾았고 에러를 해결할 수 있었다. 

어떠한 이유로 위와 같은 에러가 발생했는지는 알 수 없었지만 문제 해결 과정을 정리해본다.

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:CreateServiceLinkedRole",
            "Resource": "arn:aws:iam::*:role/aws-service-role/*"
        }
    ]
}

```

IAM에서 Role에 관한 정책을 설정해주는 것 같은데 아직까지 **IAM을 그룹 / 유저 생성 및 기존의 정책 부여**하는 식으로만 사용해봐서 자세히 이해할 수는 없었다(나중에 꼭 Role에 관한 부분을 보고 다시 정리해보자)

<hr>

**참고**

https://www.paulleasure.com/how-to/aws-elastic-beanstalk-resolved-not-authorized-to-perform-iamcreateservicelinkedrole-on-resource/


