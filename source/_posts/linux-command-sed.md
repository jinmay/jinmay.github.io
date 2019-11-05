---
title: sed 명령어와 -i 옵션으로 파일내용 수정하기
tags:
  - macos
  - linux
  - command
categories:
  - linux
  - command
date: 2019-01-29 15:30:23
---

# 명령어 sed

하이퍼레저 패브릭을 하다보면 쉘 스크립트를 수정해야 할 일들이 꽤나 많다. 그 중에 하나가 정식의 CA 서버를 건너뛰고 certificate 를 생성해 주는 부분인데 이 때에 sed 명령어를 사용하는 스크립트들이 굉장히 많다.

##### 예시

```sh
sed -i "s/DEVORG_CA_PRIVATE_KEY/${PRIV_KEY}/g" docker-compose-e2e.yaml
```



##### 사용법

**파일 내용을 수정할 때 사용한다. cat과 파이프라인을 통해서 할 수도 있지만 꽤 손이 많이 가는 작업이기 때문에 sed를 이용하여 간단하게 수정 할 수 있다.**

sed 명령어는 ed 명령어와 grep 명령어**(필터링 기능)**의 일부 기능을 합친 것이라고 한다.(sed는 Stream EDitor의 약자이다) 



##### 활용

sed 명령어를 이용하여 파일 내용의 문자를 수정해보자.

```sh
# test.txt
abcde
fgh
231
```

일단 테스트를 위해 영문 알파벳과 숫자를 내용으로 하는 test.txt 텍스트 파일을 생성했다.

abcde 문자열을 숫자로 바꾸어보자.

```sh
sed 's/abcde/12321/' test.txt

# 출력
12321
fgh
231
```

파일을 확인해보면 내용은 변하지 않은채 쉘에 출력만 해줄뿐이다. 어떻게 해야지 실제로 파일 내용을 수정할 수 있는 걸까? **-i 옵션**을 사용해야지 출력이 아닌 실제 파일에 수정사항을 반영할 수 있다.

```sh
sed -i 's/abcde/12321/' test.txt
```

ubuntu나 centos같은 리눅스에서 사용한다면 별다른 출력과 에러 없이 제대로 동작하는 것을 볼 수 있다. 하지만 macOS를 사용하고 있다면 undefined label과 같은 에러가 발생하는데 **이는 macOS에서 사용하는 sed가 다른 리눅스에서 사용중인 sed와는 다르기 때문이다.**

간단하게 말하자면 각 리눅스 배포판에서 사용하고 있는 소프트웨어의 차이로 인해서 발생하는 문제이다. macOS의 경우 man을 통해서 명령어를 살펴보면 맨 윗줄에서 아래와 같은 결과를 볼 수 있다.

```sh
# sed 매뉴얼 출력
man sed

# 결과
SED(1)                    BSD General Commands Manual                   SED(1)
```

BSD 계열의 소프트웨어라는 의미이다. 하지만 ubuntu나 centos에서 똑같이 테스트하게 되면 다른 결과를 볼 수 있다.

```sh
# sed 매뉴얼 출력
man sed

# 또는 그냥 입력
sed

# 결과
GNU sed home page: <https://www.gnu.org/software/sed/>.
General help using GNU software: <https://www.gnu.org/gethelp/>.
```

잘은 모르겠지만 우분투의 sed는 GNU sed라는 것을 알 수 있다.

**다시 정리하자면 BSD와 GNU 소프트웨어의 차이로 인해서 발생한 에러라는 것이다!!**

macOS에서 GNU sed를 사용하려면 아래와 같은 작업을 해주면 된다.

```sh
# mac에서 GNU's sed 설치
# --with-default-names 옵션은 기존의 sed이름으로 gnu-sed를 사용하겠다는 의미
brew install gnu-sed --with-default-names
```