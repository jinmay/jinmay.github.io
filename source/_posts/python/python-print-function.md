---
title: 파이썬의 print() 함수
tags:
  - python
  - print
categories:
  - python
date: 2019-07-01 16:15:43
---

# 파이썬의 print() 함수

가장 간단한 함수이지만 유용한 옵션들이 있어서 정리해 보려고한다.

#### sep

separator의 역할을 한다. 기본값으로 print()함수는 sep가 ' '이다. 즉, 하나의 스페이스를 넣어주는 것이다.

```python
# 기본값
print('a', 'b', 'c', 'd') # 'a b c d'

# sep를 ''로 변경했을 경우
print('a', 'b', 'c', 'd', sep='') # 'abcd'

# sep='@'
print('example', 'gmail.com', sep='@') # example@gmail.com
```

#### end

기본값은 \n으로써 print() 함수를 사용했을때 개행이 되는 이유이다.

```python
# 한 줄에 결과가 출력 되는 것이 아닌 두 줄에 표현이 된다
print('hello')
print('hi')

# end=' '로 변경
print('hello', end=' ')
print('hi')
# 결과
# hello hi
```

#### formatting

포매팅. 파이썬3에서 새로 생긴 format을 사용할 수 있다. 인자의 위치를 사용할수도 있고 dict처럼 key, value와 같이 사용할 수 있다.

```python
print('{} but {}'.format('first', 2)) # first but 2
print('{0} but {1} and {0}'.format('first', 2)) # first but 2 and first
print('{a} but {b}'.format(a='first', b=2)) # first but 2
```
