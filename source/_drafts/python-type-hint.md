---
title: python-type-hint
categories:
  - null
tags:
---

'[python]파이썬의 타입 힌트'

파이썬은 동적 타이핑 언어로서 코드를 작성하는 시점이 아닌 프로그램이 진행될때 자료형이 정해진다. 파이썬 버전 3.5부터 도입되기 시작되었으며 타입 힌트를 적용할 수 있는 범위가 점진적으로 넓어지고 있다.

## typing 패키지

파이썬에서 타입 힌트를 사용하기 위해 typing 모듈을 임포트해야 한다. 물론 밑의 예시처럼 \*를 써서 모든 기능을 가져오는 것은 좋지 않다. 실제 사용시에는 필요한 것만 임포트할 수 있도록 한다.

```python
from typing import *
```

## 1. 변수에 사용하기

타입 힌트가 등장했던 3.5 버전에서는 사용이 불가하다. 3.6 이상부터 사용할 수 있다. 변수명 뒤에 자료형을 적어준다. 어떻게 보면 golang과 비슷한거 같기도 하다.

```python
text: str = 'Hello world!!'
num: int = 123
```

만약 사용자가 직접 만든 클래스가 있다면 클래스의 인스턴스를 생성할때에도 적용할 수 있다.

```python
class Test():
  pass

ins_a: Test = Test()
```

## 2. 함수의 인자에 사용하기

함수의 인자와 리턴에 타입 힌트를 적용할 수 있다. 어떠한 자료형의 인자를 받고 함수의 리턴형을 유추할 수 있도록 돕는다. 아래의 예시는 문자열 인자 title과 숫자형 인자 num을 사용하며 이 함수의 리턴형은 str이어야한다.

```python
def test_func(title: str, num: int) -> str:
  pass
```

만약 특정한 반환값이 없는 함수라면 이렇게 할 수 있다.

```python
def test_func1(text: str) -> None:
  print(text)
```

## 3. 리스트 / 딕셔너리 / 튜플

리스트 / 딕셔너리 / 튜플을 타입 힌트로 표현하는 방법은 아래와 같다.

```python
num_list = List[int]
test_dict = Dict[str, int]
test_tuple = Tuple(int)
```

## 결론

파이썬 3.5부터 등장한 타입 힌트에 대해서 아주 간단하게 정리했다. 함수와 변수에서 사용되는 타입 힌트에 대해서만 나열했지만 사실은 더 많은 기능들이 존재한다. 또한 mypy라는 툴을 pip로 설치하여 같이 사용하면 정적 타입 검사기로써 사용할 수 있어 타입 힌트를 사용하는 데에 도움을 줄 수 있을 것이다.

---

[참고]  
https://item4.blog/2017-09-14/Python-Typing-with-mypy/  
https://minwook-shin.github.io/python-type-hint-typing-using-mypy/  
https://lewisxyz000.tistory.com/35
