---
title: '[DNS]ip 주소와 hosts 파일'
categories:
  - web
---

서비스를 제공하는 서버 컴퓨터에 접속을 하기 위해서는 서버의 ip 주소를 알아야 한다. 또한 서버가 클라이언트에게 서비스를 제공하려면 서비스를 받을 클라이언트 또한 ip 주소를 가져야한다.

**즉, 인터넷에 연결되어 있는 장치들은 통신을 하기 위해서 ip를 가져야한다.**(컴퓨터 뿐만 아니라 모든 장치들에 해당한다.) 이렇게 인터넷에 연결된 장치들을 host라고 부르며 네트워크에 연결되어 있는 컴퓨터 또는 장치라고 생각하면 된다.

![](https://user-images.githubusercontent.com/13075035/75006044-c76e5f80-54b3-11ea-8260-da19fbbcc11a.png)

ip주소는 숫자로 되어 있기 때문에 기억하기 어렵다는 단점이 있는데, 이를 해소하기 위해 hosts 파일을 사용한다.

![](https://user-images.githubusercontent.com/13075035/75006098-f08ef000-54b3-11ea-8224-1aef114c2133.png)

hosts 파일은 핸드폰의 전화번호부와 비슷하다고 생각하면 된다. 전화번호에 이름을 부여해서 사용하는 것처럼 컴퓨터는 hosts 파일에 ip주소에 해당하는 도메인 주소를 부여해서 저장하고 관리한다.

추후에 example.com에 접속하면 사용자의 컴퓨터는 DNS를 거치지 않고 hosts 파일에 저장되어 있는 내용을 통해 example.com이 가르키는 ip로 접속할 수 있다.

**또한 별거 아닌 것 같지만 hosts 파일의 보안에 신경써야한다.** 만약 hosts 파일이 변조되어 example.com의 ip 주소가 피싱하려고 만든 서비스의 ip주소로 바뀌면 어떻게 될까? 사용자는 example.com를 제대로 입력했다고 생각했기 때문에 정상적인 사이트로 판단하고 피싱을 당할 지도 모른다.

![](https://user-images.githubusercontent.com/13075035/75006972-fafeb900-54b6-11ea-8bb8-2a432294bff0.png)

## 정리

DNS에 대해 알아보기 전에 DNS에 비슷한 역할을 하는 hosts 파일에 대해서 알아보았다. hosts 파일은 ip주소와 도메인 주소를 매칭시켜주는 역할을 하며, 클라이언트가 서비스에 접속하기 위해서 ip 주소를 외워야 하는 불편함을 간단하게 해소시켜준다.

---

[참고]  
https://opentutorials.org/module/3421/20296
https://www.youtube.com/watch?v=viy1MV_vNXA&list=PLuHgQVnccGMCI75J-rC8yZSVGZq3gYsFp&index=2
