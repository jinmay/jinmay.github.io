---
title: 파이썬으로 알아보는 자료구조 - stack
categories:
  - data-structure
date: 2020-01-10 13:33:32
tags:
---

## Stack

- 자료의 입력과 출력이 한 쪽 끝으로 제한됨
- 나중에 들어간 데이터가 먼저 나옴(LIFO - Last In First Out)
- 후입선출

![image](https://user-images.githubusercontent.com/13075035/72049492-62471c00-3302-11ea-961f-9aea3b5d98df.png)

## 예시

- 프로세스의 메모리 구조의 스택 영역

## 용어

- push : 데이터를 스택에 넣음
- pop : 데이터를 스택에서 꺼냄
- top : 데이터의 입출력이 일어나는 끝 부분

## 장단점

### 장점

- 구조가 단순하다

### 단점

- 데이터의 최대 개수를 설정해야한다.
- 메모리의 낭비가 발생할 수 있다(최대 개수만큼 저장 공간을 확보해야하기 때문).

## 연습

### 1. 파이썬의 리스트를 통해 스택 맛보기

리스트의 append 함수와 pop 함수를 사용하면 손쉽게 스택을 구현할 수 있다.

```python
s_list = []

# push
s_list.append(1)
s_list.append(2)
s_list.append(3)

print(s_list) # [1, 2, 3]

# pop
s_list.pop() # 3
s_list.pop() # 2
```

### 2. push / pop 함수 구현

```python
s_list = []

def push(data):
    s_list.append(data)

def pop():
    _res = s_list[-1]
    del s_list[-1]
    return _res

for i in range(10):
    push(i)

pop()
pop()
pop()
```

### 3. 클래스를 이용한 구현

```python
class CustomStack():
    def __init__(self):
        self.s_list = []

    def push(self, data):
        self.s_list.append(data)

    def pop(self):
        _res = self.s_list[-1]
        del self.s_list[-1]
        return _res

st = CustomStack()
for i in range(10):
    st.push(i)

st.pop()
```
