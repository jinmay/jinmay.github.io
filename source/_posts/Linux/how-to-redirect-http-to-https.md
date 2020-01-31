---
title: '[Nginx]Nginx를 사용할때 HTTP를 HTTPS로 리다이렉션하기'
categories:
  - linux
---

웹 서버로 Nginx를 사용할때 HTTP로 들어오는 요청을 HTTPS로 리다이렉션하는 방법을 정리한다.

### 방법

80번 포트로 요청이 들어올 경우 동일한 주소이지만 HTTPS로 301 redirect 할 수 있도록 하면된다. **80번 포트일때 처리할 block은 물론 당연히 HTTPS 요청을 처리하는 block로 필요하다!**

```conf
server {
    listen 80;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com;
}
```

---

[참고]  
https://rsec.kr/?p=182
https://www.psychz.net/client/question/ko/nginx-redirect-http-to-https.html
