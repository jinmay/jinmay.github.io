---
title: [파이썬]클래스의 getter 메소드와 setter 메소드(+ private 속성)
categories:
  - python
date: 2019-11-23 21:28:40
tags:
---

```python
class Student:

  def __init__(self, name, age):
    self._name = name
    self._age = age

stu1 = Student('son', 20)
```

위와 같이 학생 클래스가 있을때 학생의 나이에 조건이 있어야 된다면 다음과 같은 코드가 필요할 것이다.

```python
class Student:

  def __init__(self, name, age):
    self._name = name
    if age <= 10:
      raise ValueError('11살 이상의 학생만 가능합니다')
    self._age = age

stu1 = Student('son', 20)
stu1 = Student('son', 8) # ValueError 발생

# __init__함수의 영향을 받지 않으므로 ValueError가 발생하지 않는다
stu1._age = 8
```

문제 될것 없어보이는 코드지만 객체를 생성하고 나서 값을 변경하게 된다면 더 이상 \_\_init\_\_ 함수의 영향을 받지 않기 때문에 문제가 될 수 있다.

getter 메소드와 setter 메소드를 구현해서 문제를 해결해보자.

## getter와 setter

```python
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
```

값을 읽기 위해 getter 메소드를, 값을 쓰기 위해 setter를 사용하면 된다.

**getter를 만들기 위해 @property 데코레이터**를 사용한다. 함수 이름은 변수명과 동일하게 작성하는 관례가 있다. 그리고 **setter를 만들기 위해 @변수.setter**를 사용한다. 마찬가지로 변수명과 동일한 함수명을 추천한다.

이렇게 클래스 내부의 변수에 \_\_(더블 언더스코어)를 덧붙여서 private 속성을 만들고 값이 필요할때 setter 메소드와 getter 메소드를 사용하는 것이 보편적인 객체지향 프로그래밍 방법 중 하나라고 한다.

@property 데코레이터를 통해 프로퍼티를 만들어 보았는데, 사실 수동으로 만드는 방법도 있다.

## 수동으로 프로퍼티 만들기

```python
class Student:

  def __init__(self, name, age):
    self.__name = name
    self.__age = age

  def get_age(self):
    return self.__age

  def set_age(self, v):
    self.__age = v

  age = property(get_age, set_age)
```

age에 대한 getter와 setter 메소드를 만들고 마지막에 age라는 이름으로 프로퍼티에 등록해주었다. 이런 방식의 특징은 get_age 메소드와 set_age 메소드를 직접 사용할 수 있다는 것이다.

```python
a = Student('son', 21)
a.get_age() # 21

a.set_age(12)
a.get_age() # 12
a.age # 12
```

get_age와 set_age가 네임 스페이스에 존재하는지 확인해보자

```python
dir(a)

# 'age',
# 'get_age',
# 'set_age'
```

만약 데코레이터를 통해 프로퍼티를 작성했다면 get_age와 set_age 함수가 없을 것이다.

---

[참고]  
패스트캠퍼스 강의
