---
title: HTTPS를 위한 let's encrypt 무료 인증서 적용하기
categories:
  - linux
---

플레이 스토어와 앱 스토어에 어플리케이션을 등록하기 위해서 충족해야 하는 몇가지 사항들이 있는데 그 중에서 가장 쉬운 방법은 백엔드 서버에 SSL을 적용하는 것이다. 우분투 서버에 let's encrypt의 무료 인증서를 적용하여 HTTPS을 사용하도록 하는 방법에 대해서 정리힌다.

> 참고로 SSL을 적용하는 방법에는 두 가지가 있는 것 같다. 첫 번째, webroot 플러그인을 사용하는 방법. 두 번째, certbot 사용하는 방법인데, 두 번째 방법인 certbot을 이용하는 것이 훨씬 간단하다.

## 1. certbot 설치

사용하는 리눅스 배포판에 따라 certbot을 설치하면 된다. apt에 certbot 저장소를 아래와 같이 추가한다. 저장소 추가 후 apt 업데이트를 해주어 반영시킨다.

```sh
sudo add-apt-repository ppa:certbot/certbot
sudo apt update
```

certbot을 설치한다.

```sh
sudo apt-get install python-certbot-nginx
```

## 2. nginx 설정

사용할 환경에 따라 nginx 설정을 해준다. certbot을 이용해 인증서를 발급받을때 해당 도메인의 80번 포트로 접속이 가능한지 보기 때문에 **80번 포트**와 **server_name**을 설정해주어야 한다.

```conf
server {
    listen 80;
    server_name domain-example.com;
}
```

nginx 설정이 잘 되어있는지 확인하고 재시작하자.

```sh
sudo nginx -t
sudo systemctl restart nginx
```

## 3. SSL 인증서 발급 받기

이전에 설치한 certbot을 이용해 ssl 인증서를 받자. **SSL 인증서를 이용할 도메인 주소 하나가 꼭 필요하다.**

```sh
sudo certbot --nginx -d domain-example.com
```

위의 커맨드를 실행하면 이것 저것 물어보는 것이 있는데 Accept / Yes를 하면 문제없이 진행할 수 있다.

**인증서 경로는 아래와 같다.**

**fullchain** -> /etc/letsencrypt/live/도메인 주소/fullchain.pem  
**privkey** -> /etc/letsencrypt/live/도메인 주소/privkey.pem

## 4. certbot 관련 기초 명령어

letsencrypt의 SSL 인증서의 유효기간은 90일이다. 따라서 90일 이후에는 갱신을 해주어야 하는데 certbot에는 갱신을 자동으로 해주는 기능도 있다.

자동 갱신이 제대로 되는지 테스트 하기 위해 아래의 명령어를 입력하자.

```sh
sudo certbot renew --dry-run
```

실제 인증서를 갱신하는 명령어는 다음과 같다. crontab을 통해 갱신을 하게 되는데 **크론탭의 경로는 /etc/cron.d/certbot**이다.

```sh
sudo certbot renew
```

또한 인증서의 만료일을 확인하고 싶다면 아래와 같이 확인하자.

```sh
certbot certificates
```

## 5. nginx에 인증서 적용

지금까지 SSL 인증서를 발급하는 과정에 에러없이 진행해왔다면 nginx에 적용할 차례이다. nginx의 세팅을 수정하자. nginx를 재시작하면 HTTPS로 접근이 가능해 질 것이다.

```conf
server {
    server_name domain-example.com;

    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    ssl_certificate /etc/letsencrypt/live/domain-example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/domain-example.com/privkey.pem; # managed by Certbot
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:MD5;
}
```

---

[참고]  
https://twpower.github.io/44-set-free-https-by-using-letsencrypt  
https://devlog.jwgo.kr/2019/04/16/how-to-lets-encrypt-ssl-renew/
