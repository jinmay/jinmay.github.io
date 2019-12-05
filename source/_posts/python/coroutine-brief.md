---
title: '[파이썬]제너레이터와 코루틴'
date: 2019-12-05 21:53:08
tags:
---

코루틴은 제너레이터를 이용해서 만들 수 있기 때문에 제너레이터와 매우 유사하다. 큰 차이점으로는, 

* 제너레이터는 데이터를 생산
* 코루틴은 생산과 소비 둘 다 가능 

하다는 것이다.

## 특징

**코루틴은 싱글 스레드 안에서 동시성을 구현**하기 위해 사용된다고 한다. 그렇기 때문에 **CPU의 컨텍스트 스위칭이 일어나지 않으며 코루틴 스케쥴링 작업의 스케쥴링 오버헤드가 매우 적다.**

asycio와 같이 동시성을 필요로하는 파이썬 패키지들에서 많이 사용되고 있다고 한다. 

코루틴의 간단한 예제를 보자.

~~~python
def coroutine1():
	print('coroutine start')
	num = yield
	print('coroutine end, value:', num)
~~~

제너레이터에서 봐왔던 패턴이랑은 조금 다른 것을 알 수 있다. **num 변수는 외부로부터 들어오는 값**이고 아래의 print를 이용해 값을 출력했다. 만약 외부에 값을 생성하고 외부로부터 값을 받고 싶다면 아래와 같이 하면 된다.

~~~python
def coroutine2():
	print('coroutine start')
	num = yield 'Coroutine!!!'
	print('coroutine end, value:', num)
~~~

먼저 외부로 값을 생성하고, 다음 루틴을 기다린다. 즉, 'Coroutine!!!' 이라는 문자열을 외부로 먼저 생성한 뒤 값이 num으로 들어 오기를 기다린다는 것이다. 외부에서 코루틴으로 값을 보내고 싶다면 아래와 같이 한다.

~~~python
a = coroutine2()
next(a)
a.send(123) # 메인 루틴에서 코루틴으로 값 주입하기
~~~

두 번째 줄에서 볼 수 있듯이 코루틴을 생성하고나서 바로 시작할 수는 없다. **yield 문에서 실행될 수 있도록 next() 함수를 꼭 실행해 주어야 한다.**

## 코루틴에 값 주입하기

send함수를 통해서 코루틴에 값을 넣을 수 있다.

~~~python
a.send(123)
a.send(456)
~~~

## StopIteration

더이상 반복할 yield가 없다면 StopIteration 예외를 발생시킨다. 

## 코루틴의 현재 상태 알아보기

inspect 모듈의 getgeneratorstate 함수를 통해 현재 어떠한 상태일 지 알아볼 수 있다. 아래의 네 단계로 나누어진다.

* GEN_CREATED - 처음 대기 상태
* GEN_RUNNING - 실행 상태
* GEN_SUSPENDED - yield 대기 상태
* GEN_CLOSED - 실행 완료 상태

~~~python
from inspect import getgeneratorstate

def co3():
	a = yield
	b = yield

test = co3()
print(getgeneratorstate(test)) # GEN_CREATED
next(test)
print(getgeneratorstate(test)) # GEN_SUSPENDED
test.send(1)
print(getgeneratorstate(test)) # GEN_SUSPENDED
test.send(2) # StopIteration 예외 발생
~~~

## 예외 처리 및 종료

throw() 함수로 예외를 던질 수 있다. 그리고 close() 함수로 코루틴을 종료할 수 있다.

~~~python
a.throw(Exception)
a.close() # GEN_CLOSED
~~~

## 데코레이터로 만들기

먼저 한 번 실행해야 한다는 불편함을 해소하기 위해 데코레이터로 만들 수 있다. 코루틴을 생성하고 next()를 한 번 해주는 데코레이터를 만들어보자.

~~~python
def coroutine(func):
	def inner(*args, **kwargs):
		cr = func(*args, **kwargs)
		next(cr)
		return cr
	return inner

@coroutine
def test_co():
	print('start')
	num = yield 'Coroutine!!!'
	print('end', num)

a = test_co()
a.send(123)
~~~

맨 위에서 작성했던 코드와 똑같이 동작한다. **차이점은 a를 생성 후 next()로 한 번 실행하지 않아도 바로 코루틴을 사용할 수 있다는 것이다.**

## yield from 키워드

파이썬 3.3 버전부터 yield from 이라는 키워드를 사용할 수 있다. **다른 제너레이터에게 작업을 위임할때 사용**하며 흐름을 넘겨받을 다른 제너레이터가 필요하다.

~~~python
def func_a():
	for item in ['a', 'b', 'c']:
		yield item

def func_b():
	yield from func_a()

aa = func_b()
next(aa)
next(aa)
next(aa)
next(aa) # StopIteration
~~~

func_b 제너레이터는 func_a 제너레이터에게 작업을 위임했다. func_a는 리스트를 순회하면서 상위 제너레이터에게 값을 전해준다. 상위 제너레이터인 func_b에 next 함수가 없다는 것이 특징이다. 

## 결론

* 제너레이터와 다르게 값을 생성하고 소비할 수 있다. -> 그래서 동시성 프로그래밍에 사용될 수 있다.
* 값을 입력 받을 수 있는 제너레이터 -> 코루틴

제너레이터는 yield 키워드를 통해 값을 메인 루틴으로 생성해 줄 수 있지만, 코루틴은 메인루틴으로 값을 생성할 뿐만 아니라 메인 루틴으로 부터 받은 값을 소비할 수 있다는 큰 차이점이 있다. 그리고 이러한 차이로 인해서 동시성 프로그래밍을 할 수 있다는 것을 알아두자!!


---
[참고]  
<https://hamait.tistory.com/830>  
<https://ddanggle.gitbooks.io/interpy-kr/ch22-Coroutines.html>  
<https://github.com/Gyubin/TIL/blob/master/Python/coroutine.md>  
<https://sjquant.tistory.com/13>  
<https://sjquant.tistory.com/14>  
<https://poppy-leni.tistory.com/entry/%EB%8F%99%EC%8B%9C%EC%84%B1-vs-%EB%B3%91%EB%A0%AC%EC%84%B1>
<https://umbum.tistory.com/445>  