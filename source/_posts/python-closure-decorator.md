---
title: 파이썬의 클로저
categories:
  - python
date: 2019-11-19 14:59:32
tags:
---

함수 안에 함수를 정의하는 내부함수를 통해 클로저를 구현할 수 있다. 파이썬이 함수를 일급 객체로 취급하기 때문에 가능하다. 클로저의 **간단한 사전적 의미로는 "일급 객체를 지원하는 언어에서 유효 범위 이름을 바인딩하는 기술"** 이라고 하는데 이해하기 복잡하니 내부함수를 반환하는 함수라고 (간단하게) 생각해도 될 것 같다.

클로저를 살펴보기에 앞서 변수의 스코프에 대해 먼저 알아보자.

## 클로저 이해에 필요한 스코프

~~~python
def func1(a):
  print(a)
  print(b)

func1(1)
~~~

너무 간단하다. b의 값이 없기 때문에 에러가 발생한다.

~~~python
b = 123
def func2(a):
  print(a)
  print(b)

func2(1)
~~~

함수 안에는 b 변수에 대한 값이 없으며 전역으로 선언된 b = 123을 사용하기 때문에 에러없이 함수를 호출할 수 있다.

~~~python
b = 123
def func3(a):
  print(a)
  print(b)
  b = 321

func3(1)
~~~

이번 예제는 주의해서 봐야한다. 일단 에러가 발생한다. 함수 안에서 변수 b를 출력하려고 하는데 값이 초기화 되기 전에 사용했기 때문에 UnboundedError가 일어난 것이다.  

조금 더 설명하자면,  
func3이 실행될때 런타임 상에서 변수의 존재를 체크한다. 함수 내에 변수 b가 있기 때문에 변수의 존재는 알고 있게되며 값이 할당되기 전에 출력을 먼저 하려고 해서 에러가 발생하게 된 것이다.


## 클로저

위의 스코프를 이해했다면 클로저의 동작 방식을 이해하는 데에도 큰 도움이 된다. 자유 변수(free variable)가 무엇인지 보자.

~~~python
def outer():
  # free variable 영역
  # 클로저 영역
  number_list = []
  def inner(v):
    number_list.append(v)
    print(sum(number_list))
  return inner

func = outer() # inner 함수 리턴 후 outer 함수 종료
func(1) # 1
func(2) # 3
func(3) # 6
~~~

함수 실행의 결과는 인자로 들어간 값들이 누적된 합이 출력된다. outer 함수는 inner 함수를 리턴하고 종료가 되었는데 어떻게 inner 함수는 outer 함수 영역에 존재하는 변수를 계속해서 사용할 수 있는 것일까?

**number_list 변수가 자유 변수 영역에 있기 때문이다.** 내부함수 입장에서 자기가 선언하지 않은 변수를 자유 변수라고 이해하면 쉬울 것 같다. 외부함수의 변수를 계속 참조할 수 있어서 값을 계속 사용할 수 있다는 장점이 있지만 남용하게 되면 메모리를 낭비할 수도 있다는 단점도 있다.

## 자유변수 확인하기

dir 함수를 사용한다면 객체가 가진 내부정보를 볼 수 있다. 자유 변수로 사용되는 변수가 무엇인지 확인하려면 아래와 같이 하면 된다.

~~~python
print(func.__code__.co_freevars)
# ('number_list',)
~~~

자유 변수로서 사용되는 변수를 출력한다. 변수의 값을 확인하려면 이렇게 하면 된다.

~~~python
print(func.__closure__[0].cell_contents)
# [1, 2, 3]
~~~

## 클로저 사용시 주의사항

~~~python
def outer():
  count = 0
  total = 0
  def inner(v):
    # nonlocal count, total # 문제 해결
    count += 1
    total += v
    print(total / count)
  return inner

func = outer()
func(1) 
# UnboundLocalError: local variable 'count' referenced before assignment
~~~

count 변수를 값이 할당 되기 전에 사용했기 때문에 에러가 발생한다. **여기서 중요한 점은 밖의 변수들과 inner의 변수들이 이름이 같을지라도 다른 변수라는 것이다.** 문제를 해결하기 위해서 nonlocal 키워드를 사용해주어야 한다.

~~~python
nonlocal count, total
~~~


---
[참고]
<https://itholic.github.io/python-closure/>  
<http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%81%B4%EB%A1%9C%EC%A0%80-closure/>  

