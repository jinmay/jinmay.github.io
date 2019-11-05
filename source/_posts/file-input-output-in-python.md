---
title: 파이썬 파일 입출력
tags:
  - python
categories:
  - python
date: 2018-04-05 16:08:05
---
기본형태 

```python
f = open('file name', 'mode')
```

f -  파일 객체

file name - 파일 문자열 이름

mode - 파일 타입과 무엇을 할지

##### mode

r - 파일 읽기

w - 파일 쓰기

x - 파일 쓰기(파일이 존재하지 않을 경우)

a - 파일 추가(파일이 존재하면 기존의 내용의 뒤에 추가)

-----------------------

**mode의 두번째 글자** - 파일타입 지정

t(또는 아무것도 명시하지 않음) - 텍스트 타입

b - 이진 타입



### 텍스트 파일 쓰기

**write()**

```sh
f = open('test.txt', 'w')
f.write('first sentence')
f.close() # 꼭 닫아주어야 한다
```

텍스트 파일을 쓰는 방법 

1. write()
2. print()

위의 예제를 **print()**을 통해서 쓰려면

```python
f = open('test.txt', 'w')
print('first sentence', file=f)
f.close()
```

기본적으로 print()는 각 인자 뒤에 스페이스를, 끝에 줄바꿈을 추가한다

print()를 write() 처럼 작동하려면 print()에 다음 두 인자를 전달해야 한다

* sep(구분자) - 기본값은 ' ' 스페이스 이다
* end(문자열 끝) - 기본값은 '\n' 줄바꿈 이다



### 텍스트 파일 읽기

**read(), readline(), readlines()**

1. read()

인자 없이 호출하여 한 번에 전체 파일을 읽을 수 있다

엄청 큰 파일을 read()로 읽으면 메모리 소비가 심하므로 주의!!

```python
f = open('test.txt', 'r')
contents = f.read()
f.close()

print(contents)
```

* 읽기 + 쓰기
  * 파일 안의 내용을 확인하여 내용을 추가하는 경우가 생길 수 있다.
  * 읽기용 파일 객체와 쓰기용 파일 객체를 따로 생성하는 것이 아니라 읽기 / 쓰기를 동시에 수행할 수 있는 파일 객체를 생성하면 된다
  * 읽기 / 쓰기를 둘 다 하고 싶을대는 **r+**모드를 사용하면 된다

```python
f = open('test.txt', 'r+')
f.read() # 모든 내용 출력
f.write('hello world') # 해당 문자열을 저장 후 글자수 리턴
f.read() # 빈 문자열 출력
```

위 와 같은 문제가 발생하는 이유는 첫 번째  read() 메소드를 사용했을 때 파일을 읽는 위치가 변했기 때문이다. read() 메소드를 사용하면 읽은 만큼 커서가 뒤로 이동했기 때문에 다시 read() 를 호출한다 한들 빈 문자열이 출력되는 것이다.  
read 메소드를 사용한 뒤 다시 파일의 처음부터 읽으려면 커서의 위치를 처음으로 옮겨주어야 하는데 **seek 메소드**를 통해서 할 수 있다.

```python
f = open('test.txt', 'r+')
f.read() # 커서 맨 뒤로 이동
f.read() # 빈 문자열 출력
f.seek(0) # 파일의 맨 앞으로 커서 이동
f.read() # 모든 내용 출력
```

2. readline()

파일을 라인 단위로 읽는다

```python
f = open('test.txt', 'r')
a = f.readline()
f.close()

print(a) # 첫번째 줄만 읽어서 출력
```

3. readlines()

한 번에 모든 라인을 읽고, 한 라인으로 된 문자열들의 리스트를 반환한다

```python
f = open('test.txt', 'r')
a = f.readlines()
f.close()

for line in a:
    print(line, end='')
```

**with를 이용한 예제**

```python
with open('sample.txt', 'w') as f:
    f.write('example sentence')
```

context manager 코드 블록의 코드가 실행이 되고나서 **자동으로 파일을 닫아준다**





### CSV

Comma-Seperated-Values

* 쓰기 예제

```python
import csv

lists = [
    ['a', 'first'],
    ['b', 'second'],
    ['c', 'third'],
    ['d', 'fourth'],
    ['e', 'fifth'],
    ['f', 'sixth']
]

with open('test2.txt', 'w') as f:
    csvout = csv.writer(f)
	csvout.writerows(lists)
```

* 읽기 예제(List)

```python
import csv

with open('test2.txt', 'r') as f:
    var = csv.reader(f)
    lists = [row for row in var]
```

* 읽기 예제(Dictionary)

```python
import csv

with open('test2.txt', 'r') as f:
    var = csv.DictReader(f, fieldnames = ['alphabet', 'number'])
    aaa = [row for row in var]
```

