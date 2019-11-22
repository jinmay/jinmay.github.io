---
title: "[파이썬]고차함수"
categories:
  - python
date: 2019-11-22 13:32:14
tags:
---

**파이썬에서 모든 것은 객체다.** 라는 유명한 설명이 있다. 파이썬은 일급 객체를 지원하는 언어이며 아래와 같은 특징을 갖는다.

- 런타임에서 초기화 가능
- 변수 등에 할당 가능
- 함수의 인자로 전달 가능
- 함수의 결과값으로 사용 가능

위와 같은 일급 객체의 특징을 가진 함수가 있다면 그것을 **고차함수(Higher Order Function)**이라고 부를 수 있으며 좀 더 특징을 다듬어 보자면

- **다른 함수를 생산/소비 하는 함수**
- **다른 함수를 인자로 받거나, 결과로 함수를 반환하는 함수**

라고 할 수 있다.

## 정말 모든 것은 객체일까?

간단한 함수를 만들고 타입을 확인하자.

```python
def func():
  pass

class A:
  pass

print(type(func), type(A))
# <class 'function'> <class 'type'>
```

출력된 값을 보면 둘 다 객체라는 것을 알 수 있다.

## 고차함수 예시

파이썬 빌트인 함수 중에서 대표적인 고차함수로 map이 있다. map 함수는 인자로서 **함수**와 **순회가능한 객체**를 받는다.

```python
number_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
result = list(map(lambda x: x ** 2, number_list))
print(result)
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

---

[참고]
AskDjango  
<https://itholic.github.io/etc-higer-order-function/>
