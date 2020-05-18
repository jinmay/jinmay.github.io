---
title: '[mysql]macOS에서 brew로 mysql 설치할때 주의할 점'
categories:
  - mysql
---

brew install 명령어를 통해 mysql을 설치할 때, 가끔 알 수 없는 에러가 발생할 때가 있다. openssl을 재설치해보고 git과 gcc 등을 다시 설치해보아도 해결 할 수 없었던 적이 있는데 생각외로 간단하게 해결할 수 있었다.

### 설치시 에러 고치기

mysql을 설치하고 나면 ```export ~~``` 하는 구문이 뜨는데, 영어라고 대충 읽고 넘어가지 말고 실행을 해주면 큰 문제 없이 작동한다.

또는,

```sh
brew install openssl

LDFLAGS=-L/usr/local/opt/openssl/lib pip install mysqlclient
```

---  
[참고]  
https://medium.com/@shandou/pipenv-install-mysqlclient-on-macosx-7c253b0112f2  
https://medium.com/@elastic7327/osx-mojave-%EC%97%90%EC%84%9C-python-%ED%8C%A8%ED%82%A4%EC%A7%80-mysqlclient%EA%B0%80-%EC%84%A4%EC%B9%98%EA%B0%80-%EC%95%88%EB%90%98%EB%8A%94-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0%EB%B0%A9%EB%B2%95-2269bcf49c33