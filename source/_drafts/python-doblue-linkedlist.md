---
title: python-doblue-linkedlist
---

파이썬으로 알아보는 자료구조 - double linked list

## Double Linked List

- 이중 연결리스트
- 단방향성을 가진 단순 연결리스트의 단점을 보완
- 이전 노드(prev)와 다음 노드(next), 그리고 데이터(data)를 가짐
- head / tail 키워드가 중요!!

## 연습

### 1. 클래스로 구현

head와 tail에 주의하여야 한다!!

```python
class Node():
    def __init__(self, data, prev=None, next=None):
        self.prev = prev
        self.data = data
        self.next = next


class NodeMgmt():
    def __init__(self, data):
        self.head = Node(data)
        self.tail = self.head

    def add(self, data):
        node = self.head
        while node.next:
            node = node.next
        new = Node(data)
        node.next = new
        new.prev = node
        self.tail = new

    def desc(self):
        node = self.head
        while node:
            print(node.data)
            node = node.next

dll = NodeMgmt(1)
for i in range(2, 11):
    dll.add(i)
dll.desc()
```

### 2. 앞 / 뒤에서부터 이중 연결리스트의 값 출력하기

```python
# 아래의 두 함수는 NodeMgmt 클래스의 인스턴스 메소드

# 앞에서부터 시작
def desc_from_head(self):
    node = self.head
    while node:
        print(node.data)
        node = node.next

# 뒤에서 시작
def desc_from_tail(self):
    node = self.tail
    while node:
        print(node.data)
        node = node.prev
```

### 3. 특정 데이터 앞에 노드 추가하기1

head에서 부터 데이터를 읽기 시작하여 특정 데이터 앞에 새로운 노드를 추가

```python
def insert_from_head(self, data, target):
    node = self.head
    while node.next:
        if node.next.data == target:
            new = Node(data)
            new.next = node.next
            node.next.prev = new
            new.prev = node
            node.next = new
            return
        else:
            node = node.next
```

### 4. 특정 데이터 앞에 노드 추가하기2

tail에서 부터 데이터를 읽기 시작하여 특정 데이터 앞에 새로운 노드를 추가

```python
def insert_from_tail(self, data, target):
    node = self.tail
    while node.prev:
        if node.data != target:
            node = node.prev
        else:
            new = Node(data)
            new.prev = node.prev
            new.next = node
            node.prev.next = new
            node.prev = new
            return
```
