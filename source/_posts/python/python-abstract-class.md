---
title: '[파이썬]추상클래스'
categories:
  - python
tags:
---

## 추상클래스란?

~~추상화된 클래스이다.~~ 상속받을 서브 클래스를 위한 클래스이다. 공통된 속성(필드, 메소드)을 가지는 것은 일반적인 클래스와 같으며 아래와 같은 차이점(특징)이 있다.

- 객체를 생성할 수 없다.
- 서브클래스에서 메소드의 구현을 강제한다.

## 예시

일반 abc 모듈이 필요하다(abc는 Abstract Base Class의 약자)

```python
import abc

# 추상클래스
class RandomMachine(abc.ABC):

  @abc.abstractmethod
  def load(self, iterable):
    pass

  @abc.abstractmethod
  def pick(self, iterable):
    pass

  def inspect(self):
    items = []
    while True:
        try:
            items.append(self.pick())
        except LookupError:
            break
    return tuple(sorted(items))

# a = RandomMachine()
# TypeError: Can't instantiate abstract class RandomMachine with abstract methods load, pick
```

추상클래스를 만들기 위해서는 abc.ABC를 상속 받아야한다. 만약 파이썬 3.4 이전 버전을 사용 중이라면 아래와 같이 상속받아야 한다.

```python
class RandomMachine(metaclass=abcABCMeta):
  pass
```

@abc.abstractmethod 데코레이터를 통해서 서브클래스에서 강제할 메소드를 만들어 주었다. 만약 메소드를 생성하지 않았다면 객체를 생성하는 시점에서 에러가 발생할 것이다.

이제 실제로 사용할 클래스를 생성해보자.

```python
import random

class NumberMachine(RandomMachine):

  def __init__(self, items):
    self._randomizer = random.SystemRandom()
    self._items = []
    self.load(items)

  def load(self, items):
    self._items.extend(items)
    self._randomizer.shuffle(self._items)

  def pick(self):
    try:
      return self._items.pop()
    except IndexError:
      raise LookupError('에러!!')

  def __call__(self):
    return self.pick()

ins = NumberMachine(range(1, 101))
ins()
```

NumberMachine 클래스를 정의했고 추상클래스에서 강제한 메소드를 구현했다. 만약 load와 pick 메소드 중에서 하나라도 구현이 되어 있지 않는다면 TypeError가 발생한다. 그리고 객체를 호출가능하게 만들기 위해 \_\_call\_\_ 특별 메소드도 구현했다.
