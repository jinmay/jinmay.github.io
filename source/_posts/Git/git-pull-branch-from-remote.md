---
title: '[git]원격 저장소의 branch 가져오기'
categories:
  - git
---

원격 저장소의 branch의 이름을 가지고 로컬로 가져오는 방법을 정리한다.

### 원격 저장소 branch를 로컬로 가져오기

```sh
git checkout -t <원격저장소 branch 이름>
```

위와 같이 입력하면 된다. 예를 들어, 원격저장소의 nickname이 `origin`이고 branch가 `git-practice`라면 아래와 같이 입력한다.

```sh
git checkout -t origin/git-practice
```

### 결론..?

로컬에서 작업하던 내용(branch)를 모두 지우고 새로 시작하고 싶을때 사용하면 좋을 것 같다.

---

[참고]  
https://cjh5414.github.io/get-git-remote-branch/
