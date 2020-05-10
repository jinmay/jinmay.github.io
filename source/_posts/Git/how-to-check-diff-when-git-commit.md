---
title: '[git]commit할 때 변경된 내용(diff) 출력하기'
categories:
  - git
---

vi를 통해 커밋 메세지를 작성할 때, 어떤 변경점이 있는 지 보고 작성하고 싶을 때가 많았다. 그래서 staging 하기 전에 *git diff* 명령어를 통해 보고 메세지를 작성하곤 했는데 *git commit*의 옵션을 통해 처리할 수 있었다. 그 방법은 간단하게 나마 정리해본다.

## git commit -v

**-v 옵션**을 사용하면 커밋할 때 변경사항들을 출력해준다. 

```sh
git commit -v
```

*git diff --staged* 명령어를 통해서도 diff를 확인할 수 있다는 점을 알아두자. 

![](https://user-images.githubusercontent.com/13075035/81502372-851d2780-9318-11ea-9bd2-2520a9bb3fae.png)
