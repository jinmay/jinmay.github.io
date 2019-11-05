---
title: AWS EC2 ubuntu 16.04에 Django runserver 배포하기
tags:
  - django
  - 장고 배포
categories:
  - django
date: 2019-05-02 16:23:54
---

# AWS EC2 ubuntu 16.04에 Django runserver 배포하기

runserver는 장고의 개발서버이다. 로컬환경에서 개발의 편의성을 위해서 기본적으로 제공해주는 서버이며 실제로 외부로 배포하는 경우는 없다. 하지만 EC2에 적응할 겸, 그리고 최종적으로 nginx + uwsgi + django 를 배포하는 과정으로 가는 과정 중 하나라고 생각하고 정리하려고 한다.

#### runserver 배포하기

개발서버인 runserver를 배포하는 방법은 크게 어렵지 않다. 
```sh
python manage.py runserver 0.0.0.0:8000
```
평소 runserver를 하듯이하고 외부 접속을 위해 **0.0.0.0** 라고 아이피를 명시해준다. 그리고 포트번호는 개발서버와 마찬가지로 8000으로 했다. 물론 포트번호도 변경 가능하다. 그리고 중요한 것이 있는데 AWS EC2에는 인스턴스들의 보안을 위해서 보안그룹이라는 기능이 있다. 해당 EC2의 인스턴스가 속한 보안그룹의 Inbound에서 TCP 8000번 포트를 열어주어야지만 외부에서 접속가능하다.