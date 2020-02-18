---
title: '[Docker]sudo 없이 docker 명령어 사용하기'
categories:
  - docker
---

맥에서 docker를 사용할땐 sudo 없이 처음부터 사용할 수 있다. 하지만 배포를 위해 리눅스 환경에서 도커를 설치하고 사용하려고 하면 mac과는 다르게 sudo 권한을 수동으로 설정해야 하는데, 이는 꽤나 귀찮은 작업이므로 해결방법에 대해서 정리한다.

## 로그인된 사용자를 docker 그룹에 등록

현재 시스템에 로그인된 사용자를 docker 그룹에 등록하면 sudo 명령어 없이 docker를 사용할 수 있다.

```sh
sudo usermod -aG docker $USER
```

## 정리

docker 데몬은 root 권한으로 실행되기 때문에 다른 사용자가 사용하기 위해서는 항상 sudo를 붙이고 사용해야 하는 귀찮음이 존재한다. root 권한으로 변경하여 사용해도 되지만 root 권한을 무분별하게 사용하는 것은 권장되지 않기 때문에 현재 로그인된 유저를 docker 그룹에 추가하는 방법이 좋다.

---

[참고]  
https://www.slipp.net/questions/485
https://blusky10.tistory.com/359
