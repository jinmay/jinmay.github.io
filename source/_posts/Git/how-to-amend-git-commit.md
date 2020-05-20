---
title: '[git]가장 최근 커밋의 커밋 메세지 수정하기'
categories:
  - git
---

commit 메세지를 수정해야 하는 상황이 자주 발생하지는 않았다. 개인적인 경험으로는 주로 커밋 메세지를 수정해야 하는 상황은 여러개의 commit을 squash한 뒤에 메세지를 수정하고 remote로 push 하는 상황이었다.

가장 최큰 commit의 메세지를 수정하는 방법에 대해서 알아보자.

### 커밋 메세지 수정하기

커밋 메세지를 수정하기 위해서는 `git commit` 명령어에 옵션이 필요하다. 이 때, 필요한 옵션은 `--amend`이다.

```sh
git commit --amend
```

`--amend` 옵션을 적고 commit을 하면 최근 커밋 메세지가 vim과 같은 편집기에 다시 한 번 나오는데, 메세지를 수정하고 저장하면 된다.

만약 remote로 이미 push를 한 상태라면 주의해야한다. 커밋 메세지를 수정하는 작업은 commit의 hash id가 달라지게 되므로 `force push`를 해주어야 하는데, 이 작업은 remote git의 commit 이력을 덮어쓰기할 수 있어서 매우 조심해야한다.

위와 같은 상황에서 remote로 push를 해야겠다면 아래와 같이 입력하자.

```sh
git push -f <remote_name> <branch_name>
```

**다시 한번 강조하지만 `force push`하는 과정은 remote git 커밋 이력을 덮어쓰기 할 수 있으므로 주의해야 한다~~**
