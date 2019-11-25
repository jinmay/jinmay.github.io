---
title: 파이썬의 이터레이터(iterator)
tags:
  - python
  - iterator
  - 파이썬
  - 이터레이터
categories:
  - python
date: 2019-05-31 14:07:45
---


# 파이썬의 이터레이터(iterator)

파이썬의 **이터레이터는 next() 라는 특별 메소드를 호출함으로써 컨테이너 안에 있는 값들을 차례대로 순회할 수 있는 객체**이다.

### iterator의 원리

**파이썬의 iter() 내장 함수는 객체의 \_\_iter\_\_ 메소드를 호출하며, \_\_iter\_\_ 메소드는 해당 컨테이너의 iterator를 리턴해야 한다. 그리고 \_\_iter\_\_로 반환된 iterator는 \_\_iter\_\_와 \_\_next\_\_를 구현해야만 한다. 역으로 iter와 next를 둘 다 구현하고 있는 객체를 iterator의 객체라고 볼 수 있다!**

### iterator의 특성

iterator 객체는 반드시 \_\_next\_\_ 메소드를 구현해야 한다. 파이썬 내장함수 iter()는 iterable 객체에 구현된 __iter__를 호출함으로써 그 iterable 객체의 iterator 객체를 리턴한다. 이때 iter() 메소드가 리턴하는 iterator는 **동일한 클래스 객체**가 될 수도있고 **따로 작성된 iterator 객체**가 될 수도 있다!!

> 주의! <br>
> 구현하는 클래스가 iterator 이려면 \_\_iter\_\_와 \_\_next\_\_를 함께 구현해야하고<br>
> 단순히 iterable 하기만 해도 된다면 \_\_iter\_\_만 있어도 된다

#### 클래스에 \_\_iter\_\_가 구현되어 있다면 iterable 객체이다.
```python
import collections

class A:
  def __iter__(self):
    return self

a = A()
isinstance(a, collections.abc.Iterable) # True
```

#### \_\_iter\_\_ 메소드는 iterator 객체를 반환해야만 한다!!

클래스를 구현하면서 __iter__ 메소드가 리턴하는 iterator에는 두 가지 방법이 있다

* iterable이자 iterator인 클래스라면 \_\_iter\_\_는 self를 리턴한다

```python
# NewRange 클래스는 iterable 이면서 iterator 이다
class NewRange:

  def __init__(self, end):
    self.start = 0
    self.end = end

  def __iter__(self):
    return self

  def __next__(self):
    if self.start < self.end:
      value = self.start
      self.start += 1
      return value
    else:
      raise StopIteration

num_list = NewRange(11)
for n in num_list:
  print(n, end=' ') # 0 1 2 3 4 5 6 7 8 9 10
```

* 단지 iterable한 클래스라면 \_\_iter\_\_ 함수는 따로 구현된 iterator를 리턴하는 객체를 리턴해야한다

```python
# 위의 NewRange 클래스에 이어서...

# newRangeFactory 클래스는 iterable 하며 내부의 __iter__ 메소드는
# iterator 객체를 리턴해야 한다
class NewRangeFactory():

  def __init__(self, end):
    self.start = 0
    self.end = end
  def __iter__(self):
    return NewRange(self.end)

a = NewRangeFactory(11)
a_iter = iter(a)

for n in a_iter:
  print(n, end=' ') # 0 1 2 3 4 5 6 7 8 9 10
```

- - - 
[참고]

<https://www.flowdas.com/blog/iterators-in-python/index.html>

<http://pythonstudy.xyz/python/article/23-Iterator%EC%99%80-Generator>