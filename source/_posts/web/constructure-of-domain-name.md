---
title: '[DNS]도메인 이름의 구조'
categories:
  - web
---

![](https://user-images.githubusercontent.com/13075035/75011772-c3e2d480-54c3-11ea-8e2d-3976d52e3a33.png)

도메인 이름은 네 가지로 나눌 수 있다.

- Root
- Top-level
- Second-level
- Sub

그리고 DNS에서 통용되는 규칙 몇 가지가 있다.

- 직속 하위 레벨의 DNS 서버의 리스트를 알고 있어야 한다.
- 모든 컴퓨터는 적어도 Root 도메인의 DNS 서버를 알고 있어야 한다.

## 도메인 서버를 찾아과는 과정

위와 같이 blog.example.com 이라는 도메인에 접속하려고 할 때, 한 번에 blog.example.com를 가진 DNS 서버를 찾는 것은 어렵기 때문에 여러 레벨의 도메인 서버를 거쳐가기 마련이다.

![](https://user-images.githubusercontent.com/13075035/75014946-d3b1e700-54ca-11ea-90cd-8794aeb7f464.png)

클라이언트가 blog.example.com의 ip 주소를 알아가는 과정을 의식의 흐름대로 정리해보면 다음과 같다.

1. Root 도메인의 DNS 서버를 알고 있으며, blog.example.com의 ip 주소를 물어본다.
2. Root 도메인 서버는 blog.example.com의 ip를 모르기 때문에 하위 레벨인 .com의 DNS 서버를 알려준다.
3. 클라이언트는 다시 .com의 DNS 서버에게 blog.example.com를 물어보지만 모르기 때문에 하위 레벨인 .example(2nd level)의 DNS 서버를 알려준다.
4. .example를 관리하는 2nd level의 DNS 서버도 blog.example.com를 모르기 때문에 blog라는 서브 도메인을 관리하는 DNS 서버를 알려준다.
5. blog를 관리하는 DNS 서버는 blog.example.com의 ip 주소를 알고 있기 때문에 클라이언트에게 알려준다.
