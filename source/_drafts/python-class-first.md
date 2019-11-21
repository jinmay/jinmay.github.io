---
title: python-class-first
categories:
  - python
---

# [파이썬]클래스 정리

## private 변수(private 네임 맹글링)

파이썬은 자바와 다르게 접근제어자가 없다. 사실 클래스 정의 외부에서 접근할 수 있도록 만드는 방법이 있긴한데 접근제어자처럼 완벽하게 통제할 수는 없다.

~~~python
class Duck():

  def __init__(self, name):
    self.__name = name

a = Duck('son')
print(a.__name)
# 'Duck' object has no attribute '__name'
~~~

더블 언더스코어를 사용하면된다. **__name을 직접 접근할 수 없다!!** 완벽한 private은 아니지만 외부 코드에서 발견할 수 없도록 이름을 맹글링(뭉게기) 했다고 보면 된다.

## getter setter.md 만들고 정리하면 좋을듯
## \_\_init\_\_ 함수에 로직 추가하기

~~~python
class Student:

  def __init__(self, name, age):
    self._name = name
    self._age = age

stu1 = Student('son', 20)
~~~

위와 같이 학생 클래스가 있을때 학생의 나이에 조건이 있어야 된다면 다음과 같은 코드가 필요할 것이다.

~~~python
class Student:

  def __init__(self, name, age):
    self._name = name
    if age <= 10:
      raise ValueError('11살 이상의 학생만 가능합니다')
    self._age = age

stu1 = Student('son', 20)

# __init__함수의 영향을 받지 않으므로 ValueError가 발생하지 않는다
stu1._age = 8
~~~

문제 될것 없어보이는 코드지만 객체를 생성하고 나서 값을 변경하게 된다면 더 이상 \_\_init\_\_ 함수의 영향을 받지 않기 때문에 문제가 될 수 있다.

getter 메소드와 setter 메소드를 구현해서 문제를 해결해보자.

## getter와 setter

~~~python
class Student:

  def __init__(self, name, age):
    self.__name = name
    if age <= 10:
      raise ValueError('11살 이상의 학생만 가능합니다')
    self.__age = age

  @property
  def age(self):
    return self.__age

  @age.setter
  def age(self, age):
    if age <= 10:
      raise ValueError('11살 이상의 학생만 가능합니다')
    self.__age = age
~~~