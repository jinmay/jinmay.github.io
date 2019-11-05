---
title: git에서 add한 파일 되돌리기
tags:
  - git
categories:
  - git
date: 2019-06-21 09:27:12
---

# git add한 파일을 다시 unstaged 상태로 되돌리기

보통 git add 명령어를 통해 커밋의 대상이 될 파일들을 staging area로 올리는 작업을 하곤 한다. 추가 되지 말아야 할 파일들까지도 add 했을때 다시 되돌리는 방법이다.

```sh
git reset -- <file> # 파일
git reset -- <folder>/ # 폴더
```

파일의 경우 파일명을 적어주면 되고, 폴더의 경우는 폴더임을 알려주기 위해 슬래시(\/)를 사용해 주면 좋다!

<hr>
[참고]

<https://stackoverflow.com/questions/4475457/add-all-files-to-a-commit-except-a-single-file>

```

```
