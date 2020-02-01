---
title: github-action-syntax
---

'[Github]깃허브의 CI툴인 Actions의 문법 간단 정리'

GitHub Actions의 Workflow syntax에 대해서 정리한다.

### workflow 예시 전체 코드

```yml
# https://help.github.com/en/actions/automating-your-workflow-with-github-actions/configuring-a-workflow
name: Greet Everyone
on: [push]

jobs:
  build:
    name: Greeting

    runs-on: ubuntu-latest
    steps:
      - name: Hello world
        uses: actions/hello-world-javascript-action@v1
        with:
          who-to-greet: 'Mona the Octocat'
        id: hello

      - name: Echo the greeting's time
        run: echo 'The time was ${{ steps.hello.outputs.time }}.'
```

### name

필수 필드 아님.

workflow의 이름을 지정할 수 있다. 이 workflow의 이름은 actions의 페이지에 나타난다. name을 지정하지 않으면 workflow 파일이 저장된 경로 이름으로 자동으로 지정된다.

```yml
name: Greet Everyone
```

### on

필수필드. workflow을 수행하게하는 github 내의 이벤트를 지정해야한다. 간단하게는 문자열로 시작해서 array의 형태로 지정할 수 있다. 사용가능한 이벤트의 종류는 [여기](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/events-that-trigger-workflows)서 확인할 수 있다.

```yml
# push일 때 수행
on: push

# push / PR일 때 수행
on: [push, pull_request]
```

#### 1. on.<push|pull_request>.<branches|tags>

특정한 브랜치일때만 수행할 수 있도록 설정도 가능하다.

```yml
on:
  # master 브랜치에 push할 때
  push:
    branches:
      - master

  # master 브랜치로 PR 할 때
  pull_request:
    branches:
      - master
```

push / PR 이벤트를 사용할때, 특정한 브랜치 또는 태그에 대해서만 workflow를 실행하도록 설정할 수 있다. branches / branches-ignore / tags / tags-ignore를 event의 glob pattern(글롭 패턴) 또한 사용할 수 있다. 자세한 필터 설정은 [여기](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#filter-pattern-cheat-sheet)를 참고.

```yml
on:
  push:
    branches:
      - master
      - 'mona/octocat'
      - 'releases/**'

    tags:
      - v1
      - v1.*
```

주의해야할 점으로는 branches / branches-ignore를 같이 쓸 수 없다. 마찬가지로 tags / tags-ignore도 같이 사용할 수 없다.

```yml
on:
  push:
    branches-ignore:
      - 'mona/octocat'
      - 'releases/**-alpha'

    tags-ignore:
      - v1.*
```

포함시켜야 하는 것과 그렇지 않은 것을 조건으로서 함께 사용하려면 느낌표(!)를 사용해야 한다.

```yml
on:
  push:
    branches:
      - 'releases/**'
      - '!releases/**-alpha'
```

#### 2. on.<push|pull_request>.paths

업데이트된 파일 경로를 이벤트 트리거로서 지정할 수도 있다. 아래의 예시는 docs/ 폴더 아래의 파일들에 대해서 작업을하고 push를 하면 workflow를 수행하지 않는다. **docs/ 폴더 밖에서 적어도 하나의 파일이 수정되었을때 push 했을 경우 workflow가 수행된다.**

```yml
on:
  push:
    paths-ignore:
      - 'docs/**'
```

#### 3. on.schedule

crontab 처럼 POSIX unix time을 활용하여 스케쥴링 할 수 있다. 스케쥴링 가능한 최소 인터벌 시간은 5분이다.

```yml
# 매 15분 마다 워크플로우 수행

on:
  schedule:
    # yml 파일에서 *는 특수 문자이기 때문에 꼭 따옴표로 감싸주어야 함
    - cron: '*/15 * * * *'
```

### env

**job과 step에서 사용가능한 환경변수의 map이다.** 모든 job과 step에 공통적으로 사용가능한 환경 변수를 지정할 수 있으며 각각의 job / step에서만 사용할 수 있는 환경 변수 설정도 가능하다(jobs.<job_id>.env과 jobs.<job_id>.step.env).

전역 / job / step의 환경 변수를 설정할 수 있는데, 일반적인 프로그래밍처럼 환경 변수의 이름이 같다면 더 작은 범위의 환경변수를 사용하게 된다.

```yml
env:
  SERVER: production
```

### job

워크플로우는 하나 이상의 job으로 이루어져있다. **job은 기본적으로 병렬적으로 수행되기 때문에 여러개의 job이 순차적으로 실행되지 않는 점에 유의해야 한다.**

#### 1. jobs.<job_id>

모든 job은 job_id를 가지고 있다. job_id는 문자열이어야 한다.

```yml
jobs:
  my_first_job:
    name: My first job
  my_second_job:
    name: My second job
```

#### 2. jobs.<job_id>.name

Github에 표시되는 job 이름이다.

#### 3. jobs.<job_id>.needs

job은 병렬적으로 실행되기 때문에 동기적으로 실행해야 한다면 needs 조건을 주어야 한다. 문자열 또는 문자열의 array타입이 값으로 사용된다. 만약 조건이 되는 job이 실패하면 그 job을 필요로하는 모든 job은 실행되지 않는다.

```yml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    needs: [job1, job2]
```

#### 4. jobs.<job_id>.runs-on

**필수필드이다.** job이 실행될 머신의 타입을 의미한다. 머신의 타입에는 GitHub-hosted runner와 self-hosted runner 두 가지 타입이 있다. GitHub-hosted runner를 사용하면 아래의 가상환경을 사용할 수 있다. 중요한 점으로 각각의 job은 독립된 가상환경으로 구분되어 있다는 것이다. 즉, job1과 job2는 서로 다른 도커 컨테이너에서 구동된다고 생각하면 된다.

![https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idruns-on](https://user-images.githubusercontent.com/13075035/73546396-82d94080-4480-11ea-87a9-9ccb0e134513.png)

```yml
runs-on: ubuntu-latest
```

#### 5. jobs.<job_id>.env

job_id에서 사용할 환경 변수이다. job 아래의 모든 step에서 사용가능하고 전역적으로 선언된 env를 오버라이딩한다.

#### 6. jobs.<job_id>.if

job을 수행하는 조건을 지정한다. Actions에서 사용되는 표현식은 [여기](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/contexts-and-expression-syntax-for-github-actions)에서 참고하자.

```yml
jobs:
  build:
    if: github.base_ref == 'master'
```

#### 7. jobs.<job_id>.steps

job은 step이라고 불리는 일련의 작업들을 가진다. step은 리눅스 명령어를 실행할 수 있을 뿐만 아니라 다른 action 등을 실행할 수 있다. step은 자체 프로세스 안에서 수행되기 때문에 각 step 사이에서의 환경 변수의 변화는 유지되지 않는다.

```yml
name: Greeting from Mona

on: push

jobs:
  my-job:
    name: My Job
    runs-on: ubuntu-latest
    steps:
      - name: Print a greeting
        env:
          MY_VAR: Hi there! My name is
          FIRST_NAME: Mona
          MIDDLE_NAME: The
          LAST_NAME: Octocat
        run: |
          echo $MY_VAR $FIRST_NAME $MIDDLE_NAME $LAST_NAME.
```

#### 8. jobs.<job_id>.steps.if

각 step은 job 처럼 실행 조건을 가질 수 있다.

```yml
steps:
  - name: My first step
    if: github.event_name == 'pull_request' && github.event.action == 'unassigned'
    run: echo This event is a pull request that had an assignee removed.
```

#### 9. jobs.<job_id>.steps.run

os의 shell을 통해 커맨드라인 명령어를 실행한다. step의 name을 명시하지 않았다면 run 의 내용이 자동으로 step의 이름으로 지정된다.

```yml
# single line
- name: Install Dependencies
  run: npm install

# multi line
- name: Clean install dependencies and build
  run: |
    npm ci
    npm run build
```

working-directory 키워드를 사용해서 쉘 명령어가 실행될 디렉토리도 지정 가능하다.

```yml
- name: Clean temp directory
  run: rm -rf *
  working-directory: ./temp
```

쉘을 변경할 수 있다.

```yml
steps:
  - name: Display the path
    run: echo $PATH
    shell: bash
    # python / pwsh / cmd / powershell
```

#### 10. jobs.<job_id>.steps.env

step안에서 사용할 환경 변수를 만든다.

```yml
steps:
  - name: My first action
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      FIRST_NAME: Mona
      LAST_NAME: Octocat
```

---

[참고]
https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#name
