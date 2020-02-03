---
title: '[Django]쉘 스크립트에서 collectstatic 실행시 yes 입력하기'
categories:
  - django
---

새로운 코드를 받아오고 반영하기 위한 일련의 과정들을 자동 배포를 위한 쉘 스크립트에서 관리하고 있다. 하지만 Django의 collectstatic 명령어는 사용자로부터 yes / no 입력을 받아야지 작업을 마무리 하는데 쉘 스크립트에서는 사용자가 입력을 할 수 없어 에러가 발생하는 경우가 종종 생긴다.

사용자의 인터렉션 없이 yes를 입력하기 위해 다음과 같이 입력한다.

### 리눅스 파이프라인 이용

리눅스의 파이프라인을 통해 손쉽게 해결할 수 있다.

```sh
# yes
echo yes | python manage.py collectstatic

# no
echo no | python manage.py collectstatic
```

### --no-input 옵션

사실 input을 받지 않도록 하는 방법도 있는데 collectstatic과 같이 기본값이 없는 경우에는 사용할 수 없다는 단점이 존재한다.

```sh
python manage.py collectstatic --no-input
```

---

[참고]  
https://stackoverflow.com/questions/8705305/automated-django-receive-hook-on-server-respond-to-collectstatic-with-yes
