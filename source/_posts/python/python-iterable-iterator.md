---
title: 파이썬3의 iterable과 iterator
tags:
  - python
  - iteration
  - iterator
  - iterable
categories:
  - python
date: 2019-05-29 15:20:27
---

# 파이썬의 iterable과 iterator

## iterable

iterable은 반복 가능한 것을 의미한다. 대표적인 iterable한 타입은 **list, tuple, set, dict, str, bytes, range**가 있다. 즉, 반복 연산이 가능하며 요소를 하나씩 리턴할 수 있는 객체를 의미한다.

**iterable 객체 - 반복 가능한 객체**

> range()도 iterable 하기 때문에 for \_ in range(1, 11): 과 같은 표현이 가능했던 것이다.

객체가 iterable 한지 판별하기 위해서 collections.Iterable의 인스턴스인지 확인해보면 된다.

```python
import collections

first = range(1, 11)
isinstance(first, collections.Iterable) # True

# 파이썬 3.8부터 collections가 deprecated 되기 때문에
# collections.abc를 import 하는게 좋다
import collections.abc

second = 345
isinstance(second, collections.abc.Iterable) # False
```

## iterator

iterator는 next() 함수로 데이터를 순차적으로 호출 할 수 있는 객체이다. next()로 데이터를 순회하다가 더 이상 멤버가 존재하지 않을 시 StopIteration 예외를 발생시킨다.

iterator를 만드는 방법에는 두 가지가 있다.

- iterable한 객체에 파이썬 내장함수 iter()를 사용해서 만든다.

```python
a = [1, 2, 3]
a_iter = iter(a)
type(a_iter) # list_iterator
```

- iterable 객체의 메소드를 사용한다.

```python
b = 1, 2, 3
dir(a)
# ['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'count', 'index']

b_iter = b.__iter__()
type(b_iter) # tuple_iterator
```

**다시 말해, iterator는 iterator protocol를 준수해야하며 \_\_iter\_\_와 \_\_next\_\_ 메소드가 있어야 한다!!**

그렇다면 list는 iterator 객체일까?

list가 iterator인지 확인해보기 위해 next() 메소드를 호출해보자

```python
a = [1, 2, 3]
next(a)
# TypeError: 'list' object is not an iterator
```

list 객체는 iterator가 아니라고 TypeError가 발생한다. 즉 파이썬의 list는 iterator가 아니다. iterator가 아니지만 어떻게 데이터를 순회하면서 차례대로 요소를 반환할 수 있는 것일까?

```python
a = [1, 2, 3]
for n in a:
  print(n)
# 1
# 2
# 3
```

iterator가 어떻게 동작하는 지 이해한다면 for 루프의 작동방법을 이해하는 데에도 도움이 된다.

```python
num_list = [1, 2, 3, 4, 5]
for n in num_list:
  print(n, end=" ") # 1 2 3 4 5
```

num_list는 list이기 때문에 iterator가 아니며 순회하면서 값을 하나씩 반환할 수 없다고 생각할 것이다. 하지만 for문은 **for \* in iterable** 형태로써 받은 iterable 객체의 \_\_iter\_\_을 호출해서 임시적으로 iterator를 생성 후 사용하기 때문에 iterator가 아닌 객체도 for 루프에서 사용 가능한 것이다.

> **iterable 객체이지만 iterator 객체는 아닐 수도 있음에 주의하자!!!!!!**

## iterator가 데이터를 순회하며 값을 하나씩 반환한다는 것의 의미

iterator는 \_\_iter\_\_와 \_\_next\_\_가 구현되어 있는 객체를 말하고 \_\_next\_\_ 메소드를 사용함으로써 데이터를 순회하면서 요소 하나씩을 리턴할 수 있다.

```python
a = [1, 2, 3]
a_iter = iter(a)

next(a_iter) # 1
next(a_iter) # 2
next(a_iter) # 3
next(a_iter) # StopIteration

a_iter2 = a.__iter__()

a_iter2.__next__() # 1
a_iter2.__next__() # 2
a_iter2.__next__() # 3
a_iter2.__next__() # StopIteration
```

> 짧게 정리해보자면,
>
> **list와 같은 컨테이너는 iterable 객체이지만 iterator 객체는 아니다. 즉, 반복 가능하지만 iterator 객체는 아닐수도 있다는 점이다!!!**

---

[참고]
<https://kkamikoon.tistory.com/91>
<https://python.bakyeono.net/chapter-7-4.html>
<https://github.com/python/cpython/blob/master/Objects/listobject.c>
<https://bluese05.tistory.com/55>
<http://pythonstudy.xyz/python/article/23-Iterator%EC%99%80-Generator>
<https://hackersstudy.tistory.com/9>
<https://ziwon.dev/post/python_magic_methods/>
<https://suwoni-codelab.com/python%20%EA%B8%B0%EB%B3%B8/2018/03/07/Python-Basic-Iterable-iterator/>
<https://offbyone.tistory.com/83>
<https://haerakai.tistory.com/34>
<https://www.flowdas.com/blog/iterators-in-python/index.html>
<https://mingrammer.com/translation-iterators-vs-generators/>
<https://dojang.io/mod/page/view.php?id=2412>
<https://www.youtube.com/watch?list=PLa9dKeCAyr7iWPMclcDxbnlTjQ2vjdIDD&v=1jo5dj672-M>
<https://dojang.io/mod/page/view.php?id=2412>
