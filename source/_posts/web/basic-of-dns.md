---
title: '[DNS]DNS의 기초적인 작동방식'
categories:
  - web
---

DNS는 Domain Name System의 줄임말로써 도메인을 관리하는 시스템이다. DNS의 기본적인 작동방식에 대해서 정리한다.

## 1. DNS 서버에 ip 등록

**서비스 제공자는 ip를 DNS 서버에 입력해달라고 요청한다.** 즉, **"도메인 example.com은 ip 93.184.216.34를 의미한다"라고 저장해달라고 요청**한다. 그리고 DNS 서버는 해당 정보를 저장한다.

![](https://user-images.githubusercontent.com/13075035/75007854-b9234200-54b9-11ea-83ec-721b621e5d03.png)

## 2. DHCP를 통해 DNS 서버 주소 할당

DHCP를 통해 클라이언트는 ip 뿐만 아니라 DNS 서버의 주소 / 서브넷 마스크 / 기본 게이트워이 등의 네트워크 정보를 할당 받는다. **DHCP는 컴퓨터가 네트워크에 연결되는 순간 바로 실행되어 네트워크의 정보를 할당 받게 되며, 따라서 어떤 DNS 서버를 사용해야 하는지도 알게 된다.**

[참고](http://www.ciokorea.com/tags/25731/DHCP/39337)

## 3. DNS로 도메인에 해당하는 ip 요청

DHCP를 통해 DNS 서버를 알고있기 때문에 접속할 도메인의 ip가 무엇인지 물어본다. 즉, DNS 서버는 example.com의 ip주소인 93.184.216.34를 클라이언트에게 알려준다.

![](https://user-images.githubusercontent.com/13075035/75008872-8d558b80-54bc-11ea-8650-7c41f1941ffa.png)

example.com의 ip 주소를 알게된 클라이언트는 ip 주소를 통해 해당 서비스에 접속할 수 있다.

![](https://user-images.githubusercontent.com/13075035/75009128-34d2be00-54bd-11ea-8953-7cf9480cbf42.png)

## DNS 이용의 장점

- hosts 파일을 관리하는 수고를 덜어준다.
- 도메인의 ip 주소가 변경되어도 DNS 서버에서만 바꾸면된다.

## 정리

- 클라이언트가 네트워크에 연결되는 순간 DHCP가 작동하여 ip를 물어볼 DNS 서버를 알게된다.
- 네이버에 접속하는 순간 다음의 두 과정을 거친다.

  1. DNS 서버에게 naver.com의 ip 주소를 물어본다.
  2. DNS 서버로부터 naver.com의 ip 주소를 받고 해당 ip로 접속한다.

---

[참고]  
https://www.youtube.com/watch?v=iM07I1X7qkg&list=PLuHgQVnccGMCI75J-rC8yZSVGZq3gYsFp&index=6  
https://jwprogramming.tistory.com/35  
http://www.ciokorea.com/tags/25731/DHCP/39337
