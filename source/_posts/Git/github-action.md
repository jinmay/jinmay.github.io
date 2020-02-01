---
title: '[Github]깃허브 액션이란'
categories:
  - git
---

![https://github.com/features/actions](https://user-images.githubusercontent.com/13075035/73540557-98943900-4473-11ea-8839-82d2bb798645.png)

Github에서 CI를 위한 툴인 Actions을 제공하고 있다. CI란 Continuous Integration의 약자로써 계속적 통합(?)이라고 번역되기도 하는데 각각의 개발자가 개발한 코드를 하나의 저장소에서 통합하는 과정을 말한다. **하지만 단순히 코드의 통합만을 의미하진 않으며 정적 분석, 단위 테스트, 분석결과 리포팅, 배포 서버에 push 하는 등의 여러 작업들을 수행한다.** 즉, 배포 전에 한 번에 모아서 통합하는 것이 아닌 조금씩 자주 계속해서 통합해 나가는 과정이라고 이해할 수 있을 것 같다.

기존의 Circle CI / Travis CI / Jenkins CI와 같은 서비스 또는 설치형 CI처럼 Github에서도 Actions이라는 CI툴을 선보였으며 별다른 복잡한 절차 없이 Github를 통해 사용할 수 있다는 장점이 있다.

### 주요 개념

Github Actions에는 몇몇의 중요한 개념이 있다.

- workflow
- event
- job
- step

#### 1. workflow

github repository에 대해 일련의 작업들을 수행하는 자동화된 프로세스이다. repository의 root에서 ./github/workflows/ 아래에 workflow에 대한 yml 형식의 설정 파일이 있어야한다.

#### 2. event

workflow 실행되도록 만드는 trigger 역할을 담당한다. push와 pull request 같은 트리거가 있다.

#### 3. job

job은 여러 개의 step으로 이루어질 수 있으며 단일한 가상 환경을 가진다.

#### 4. step

job 안에서 순차적으로 실행되는 프로세스 단위이다.

아래의 그림은 간단한 workflow의 예시이다.

```yml
# workflow
name: TEST CI

on: # event
    pull_request:
        branches:
            - master
            - develop

jobs:
    build: # job
        runs-on: ubuntu-latest

        steps: # step
            - uses: actions/checkout@v2

            - name: Set up Python 3.7
                uses: actions/setup-python@v1
                with:
                python-version: 3.7
```

Actions은 CI로서의 역할을 한다고 볼 수 있기 때문에 CD(Continuous Delivery) 작업을 함께하여 배포 파이프라인을 구축할 수 있다.
![https://www.slideshare.net/JaeyeolLee4/github-heroku-circle-ci-django-application](https://user-images.githubusercontent.com/13075035/73540395-26235900-4473-11ea-83d2-9c480244639b.png)

### 마무리

Github에서 제공하는 CI Tool로서 Actions에 대해서 아주 간단히 정리했다. CI는 작업을 단위별로 나누어 조금씩 자주 통합하여 통합과정에서 발생할 수 있는 부담을 최소화하기 위해 사용한다. 또한 Actions의 중요 개념으로 workflow / event / job / step을 간단하게 정리했다.

---

[참고]
https://asfirstalways.tistory.com/303
https://velog.io/@adam2/Github-Action-%EC%A3%BC%EC%9A%94-%EB%AC%B8%EB%B2%95
https://coffeewhale.com/cicd/github/2019/10/14/github-actions/
https://www.slideshare.net/JaeyeolLee4/github-heroku-circle-ci-django-application
