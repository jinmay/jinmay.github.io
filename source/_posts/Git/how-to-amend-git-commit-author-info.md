---
title: 이미 커밋된 로그에서 작성자(author) 변경하기
categories:
  - git
---

이미 작성된 commit일지라도 커밋 메세지와 작성자를 모두 수정하는 방법이 있다. 이번 포스팅에서는 가장 최신 커밋의 작성자(author)를 변경하는 방법을 알아보자.

### 가장 최신 커밋의 author 정보 변경

커밋 메세지를 수정하기 위해 ```git commit --amend ```를 사용할 수 있는데, 이 명령어를 응용하면 된다. 

```sh
git commit --amend --author="Author Name <email@address.com>"
```

유의해야할 점으로는 **가장 최신의 커밋만**을 대상으로 작용한다는 것이다.  

```git rebase -i``` 명령어를 사용하면 다수의 커밋을 대상으로 작업을 할 수 있는데 이는 추후에 다시 정리하자.


---
[참고]  
https://stackoverflow.com/questions/3042437/how-to-change-the-commit-author-for-one-specific-commit   
