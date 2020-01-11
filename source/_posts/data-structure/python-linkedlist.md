---
title: 파이썬으로 알아보는 자료구조 - linked list
categories:
  - data-structure
---

## Linked List

- 연결리스트
- 배열은 순차적으로 연결된 공간에 연속적으로 데이터를 저장
- 연결리스트는 떨어진 공간에서도 사용할 수 있다
- 파이썬에서는 리스트가 연결리스트를 모두 지원

![image](https://user-images.githubusercontent.com/13075035/72051986-9cff8300-3307-11ea-913e-f7fee276bf18.png)  
출처: https://medium.com/tanay-toshniwal/linked-list-introduction-662f7973dee5

## 예시

- 파이썬의 기본 자료구조인 리스트(list)

## 용어

- 노드(Node): 데이터의 저장 단위. 데이터와 포인터로 구성
- 포인터(Pointer): 각 노드 안에서 다음 노드의 주소 정보를 가지고 있는 공간

- head: 연결리스트의 맨 앞 노드
- tail: 연결리스트의 맨 마지막 노드

## 장단점

### 장점

- 동적으로 메모리 사용
- 데이터의 재구성 용이

### 단점

- 특정 인덱스의 데이터에 접근하기 어려움
- 즉, 중간 노드의 탐색이 어려움
- 연결을 위한 포인터와 같은 별도의 공간이 필요함으로 저장 공간 효율이 좋지 않음
- 중간 데이터의 삭제 / 수정 / 추가 시, 앞뒤 노드를 재구성해야 하는 추가작업 필요

## 연습

### 1. 간단히 구현

파이썬의 클래스를 활용. 각 노드를 객체로 관리한다.

```python
class Node():
    def __init__(self, data, addr=None):
        self.data = data
        self.next = addr
```

**맨 뒤에 새로운 데이터 추가하기**

```python
node1 = Node(1)
node2 = Node(2)
node1.next = node2
head = node1 # 연결리스트의 시작을 알기 위해서 보통 사용함
```

**함수를 정의하여 데이터 추가하기**

```python
def add(data):
    node = head
    while node.next:
        node = node.next
    node.next = Node(data)

node1 = Node(1)
head = node1
for i in range(2, 6):
    add(i)
```

**연결리스트의 모든 노드 출력**

```python
head = node1
def pprint():
    node = head
    while node.next:
        print(node.data)
        node = node.next
    print(node.data)

pprint()
# 1
# 2
# 3
# 4
# 5
```

### 2. 클래스 이용

연결리스트에서는 리스트의 맨 앞을 가리키는 head가 매우 중요한 것 같다.

```python
class Node():
    def __init__(self, data, addr=None):
        self.data = data
        self.next = addr

class NodeMgmt():
    def __init__(self, data):
        self.head = Node(data)

    def add(self, data):
        node = self.head
        while node.next:
            node = node.next
        node.next = Node(data)

    def desc(self):
        node = self.head
        while node:
            print(node.data)
            node = node.next

node1 = NodeMgmt(1)
for i in range(2, 11):
    node1.add(i)
node1.desc()
```

### 3. 노드 삭제

연결리스트의 노드를 삭제하는 종류에는 세 가지가 있다.

- head 삭제
- tail 삭제
- 중간 노드 삭제

```python
# 인스턴스 메소드로 추가
def delete(self, data):
    # 1. head 삭제
    if self.head.data == data:
        temp = self.head
        self.head = self.head.next
        del temp
    # 2. tail 삭제
    # 3. 중간 노드 삭제
    else:
        node = self.head
        while node.next:
            if node.next.data == data:
                temp = node.next
                node.next = node.next.next
                del temp
            else:
                node = node.next
```

### 4. 노드 찾기

특정 값을 가지는 노드 찾기

```python
# 인스턴스 메소드로 추가
def find(self, data):
    node = self.head
    while node:
        if node.data == data:
            return node.data
        else:
            node = node.next
```
