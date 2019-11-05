---
title: django의 secret_key 분리해서 관리하기
categories:
  - django
date: 2019-10-12 16:29:04
tags:
---

# django의 secret_key 분리해서 관리하기

장고의 secret_key는 보안에 있어서 매우 중요한 역할을 한다. 해쉬 값을 만들때 사용되는 등 장고의 전반적인 보안과 밀접한 관계를 가지고 있기때문에 외부에 노출이 되어서는 안된다.

50자의 랜덤한 문자로 이루어져 있고, 만약 오픈 해놓은 프로젝트의 비밀 키가 노출되었다는 생각이 들면 바꾸는 것이 좋을 것 같다.

> [여기](https://www.miniwebtool.com/django-secret-key-generator/)를 통해 50자의 새로운 비밀키를 생성할 수 있다.

### 비밀 키 관리방법 두 가지

비밀 키를 관리하는 방법에는 보통 두 가지가 있다.

1. 환경 변수 패턴: secret_key를 환경 변수로 관리
2. 비밀 파일 패턴: secret_key를 별도 파일로 관리

## 환경 변수 패턴

프로세스가 동적으로 참조하는 환경 변수에 비밀 키를 등록하여 직접적인 코드로부터 분리하는 방법이다. 

방법은 꽤 단순하다. 사용하는 쉘의 설정파일에 등록해주면 된다.

~~~sh
# vi ~/.zshrc
export DJANGO_SECRET_KEY='비밀 키 입력'
~~~

터미널 재실행 또는 source를 통해 쉘 설정파일을 반영한다. 그리고 제대로 환경 변수가 등록 되었는지 확인하기 위해 출력해보자.

~~~sh
source ~/.zshrc

# 값 출력해보기
echo $DJANGO_SECRET_KEY
~~~

echo의 결과로 비밀 키가 출력된다면 환경 변수로 저장이 잘 된것이다. 이렇게 저장한 환경 변수를 장고의 settings.py의 SECRET_KEY의 값으로 지정해주면 된다. os 모듈을 임포트하고 environ 함수를 통해 접근할 수 있다.

~~~python
# vi 장고의 settings.py
import os

SECRET_KEY = os.environ["DJANGO_SECRET_KEY"]
~~~

또는 예외 처리를 할 수 있도록 함수로 만들어서 적용하는 방법도 좋다.

~~~python
# vi settings.py
import os
from django.core.exceptions import ImproperlyConfigured

def get_env_variable(key):
    try:
      return os.environ[key]
    except KeyError:
      error_msg = f"Set the {key} environment variable"
      raise ImproperlyConfigured(error_msg)

SECRET_KEY = get_env_variable("DJANGO_SECRET_KEY")
~~~

runserver에 문제가 없다면 성공한 것이다. 

개인적으로는 환경 변수를 통해서 비밀 키값을 관리하고 있었는데 mailgun 서비스를 사용하기 위해 설정하던 중 환경 변수로 등록한 값들을 참조하지 못하는 일이 발생했다. 아파치를 사용하고 있지는 않았지만 간혹 이런 경우가 발생한다고 해서 두 번째 방법인 **비밀 파일 패턴 방식**으로 관리 방법을 바꾸었다.

## 비밀 파일 패턴

json / xml / yaml과 같은 파일에 외부에 노출되어서는 안되는 값을 분리하고 settings.py에서 참조할 수 있도록 하는 관리 방법이다.

이름이 secrets인 json 파일을 만들어서 key: value 방식으로 장고에서 사용할 세팅 값들을 저장하면 된다.

~~~sh
# vi secrets.json

{
  "DJANGO_SECRET_KEY": "비밀 키 값 입력",
  "EMAIL_HOST_PASSWORD": "비밀번호"
}
~~~

첫 번째 방법과 마찬가지로 함수를 하나 만들어서 참조할 수 있도록 해보자.

~~~python
import os
import json
from django.core.exceptions import ImproperlyConfigured

# 프로젝트 루트로부터 secrets.json 파일 경로 찾기
secret_file = os.path.join(BASE_DIR, 'secrets.json')

with open(secret_file) as f:
    secrets = json.loads(f.read())

def get_env_variable(key):
    try:
        return secrets[key]
    except KeyError:
        error_msg = f"Set the {key} environment variable"
        raise ImproperlyConfigured(error_msg)

SECRET_KEY = get_env_variable("DJANGO_SECRET_KEY")
~~~

**물론 git과 같은 버전관리시스템이 추적하면 안되기 때문에 secrets.json 파일은 .gitignore에 등록해야 한다!!**

---
[참고]
책 - Two Scoops of Django

<https://wayhome25.github.io/django/2017/07/11/django-settings-secret-key>

