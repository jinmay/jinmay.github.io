---
title: pip로 설치한 모든 패키지 삭제하기
tags:
  - python
  - pip
  - pip remove
categories:
  - python
date: 2019-04-11 17:25:22
---

# pip로 설치한 모든 패키지 삭제하기

요즘들어서 가상환경 내에 설치한 패키지들을 다 삭제하고 재설치해야 할 일이 많아졌다. 어려운 작업은 아니지만 정리해두자.

~~~sh
pip freeze | xargs pip uninstall -y
~~~

The end..