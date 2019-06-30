---
title: 파이썬 리스트에서 빈 문자열인 원소 제거하기
tags:
  - python
  - list
categories:
  - python
date: 2019-06-30 14:45:09
---


> 파이썬에서 리스트에서 빈 문자열인 원소를 제거해보자

#### 방법 1

list comprehension을 조건문과 사용해서 이용할 수 있다

~~~python
sample_list = ['', 'a', '', 'abc', 'qdsf']
sample_list = [v for v in sample_list if v]
sample_list # ['a', 'abc', 'qdsf']
~~~

#### 방법 2

join()과 split() 함수 이용

~~~python
sample_list = ['', 'a', '', 'abc', 'qdsf']
sample_list = ' '.join(sample_list).split()
sample_list # ['a', 'abc', 'qdsf']
~~~

#### 방법 3

filter() 함수 이용

**filter() 함수의 결과는 iterator이기 때문에 list 함수를 써서 리스트로 만들어 주어야 한다.**

~~~python
sample_list = ['', 'a', '', 'abc', 'qdsf']
sample_list = list(filter(None, sample_list))
sample_list # ['a', 'abc', 'qdsf']
~~~

아래와 같이 None 뿐만 아니라 다른 것도 사용할 수 있지만 None과 boolean 타입이 제일 빠르다고 한다.

~~~python
sample_list = list(filter(bool, sample_list))
sample_list = list(filter(len, sample_list))
sample_list = list(filter(lambda v: v, sample_list))
~~~

---
[참고]
<https://stackoverflow.com/questions/3845423/remove-empty-strings-from-a-list-of-strings>
<https://www.geeksforgeeks.org/python-remove-empty-strings-from-list-of-strings/>
