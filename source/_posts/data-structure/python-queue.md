---
title: 파이썬으로 알아보는 자료구조 - queue
categories:
  - data-structure
date: 2020-01-09 15:14:12
tags:
---

스택(Stack)과 더불어 가장 기초가 되는 자료구조이다.

## Queue

- 매표소에서 한 줄로 서서 기다리는 것과 같다
- 먼저 들어간 데이터가 먼저 나옴(FIFO - First In First Out)
- 즉, 선입선출

![image](https://user-images.githubusercontent.com/13075035/72041863-070c2e00-32f0-11ea-8b64-5c0f27e34f69.png)

## 예시

- 운영체제(OS)에서 프로세스 스케쥴링을 구현하는데에 사용

## 용어

- enqueue - 큐에 데이터를 삽입(insert)
- dequeue - 큐에서 데이터를 꺼냄(delete)

## 연습

### 1. 파이썬의 리스트를 이용해 enqueue, dequeue 구현

```python
q_list = []

def enqueue(data):
    q_list.append(data)

def dequeue():
    res = q_list[0]
    del q_list[0]
    return res
```

### 2. 파이썬의 클래스 이용

```python
class CustomQueue():
    def __init__(self):
        self.q_list = []

    def enqueue(self, data):
        self.q_list.append(data)

    def dequeue(self):
        _res = self.q_list[0]
        del self.q_list[0]
        return _res

    def pprint(self):
        for data in self.q_list:
            print(data)

if __name__ == '__main__':
    q = CustomQueue()
    for i in range(10):
        q.enqueue(i)

    q.pprint()

    for _ in range(5):
        q.dequeue()

    q.pprint()
```
