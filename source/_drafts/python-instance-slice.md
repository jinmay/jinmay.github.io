---
title: python-instance-slice
categories:
  - python
---

# '[파이썬]객체 슬라이싱과 __getitem__'

파이썬의 리스트와 튜플에 [](대괄호)를 이용해서 슬라이싱을 해보았을 것이다. 뿐만 아니라 리스트처럼 클래스의 인스턴스 자체도 슬라이싱을 할 수 있도록 만들 수 있다. 이때에 필요한 속성이 \_\_getitem\_\_이라는 속성이며 슬라이싱을 구현하는 방법에 대해서 정리해보자.

## \_\_getitem\_\_

두개의 밑줄로 시작하는 파이썬의 특별 메소드 중 하나이다. **슬라이싱을 구현할 수 있도록 도우며 리스트에서 슬라이싱을 하게되면 내부적으로 \_\_getitem\_\_ 메소드를 실행한다는 점**이 중요하다. 따라서 객체에서도 **슬라이싱을 하기 위해서는 \_\_getitem\_\_ 메소드가 필수적**이다.

## 코드로 살펴보는 \_\_getitem\_\_

~~~python
class CustomNumbers:
  def __init__(self):
    self._numbers = [n for n in range(1, 11)]

a = CustomNumbers()
~~~

위의 클래스를 통해 a는 1부터 10까지 저장되어 있는 리스트를 속성으로 가진다. 만약 현재의 상태에서 리스트 슬라이싱을 하고 싶다면 _numbers 속성에 직접 접근해서 사용해야할 것이다.

~~~python
a._numbers[2:5]
# [3, 4, 5]
~~~

**인스턴스 변수에 직접 접근하지 말고 객체 자체를 통해서 슬라이싱을 구현하기 위해서는 __getitem__ 특별 메소드를 정의해야한다.** 그리고 이 함수는 인덱스를 인수로 받아야 한다.

~~~python
class CustomNumbers:
  def __init__(self):
    self._numbers = [n for n in range(1, 11)]

  def __getitem__(self, idx):
    return self._numbers[idx]

a = CustomNumbers()
a[2:7]
# [3, 4, 5, 6, 7]
~~~

구현 내용은 굉장히 간단하다. 슬라이싱을 사용할 속성에 idx만 적어주면 알아서 해결해준다. 

# 정리

슬라이싱을 구현하기 위해서 필요한 것은 \_\_getitem\_\_라는 특별 메소드임을 잊지말자!

---
[정리]  
패스트캠퍼스 강의