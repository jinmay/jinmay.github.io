---
title: '[파이썬]for 반복문의 다앙한 사용법'
categories:
  - null
date: 2020-01-07 15:46:46
tags:
---

파이썬의 for 반복문은 매우 유용하다. for문에서 쓰일 수 있는 패턴을 간단히 정리한다.

## 1. 기본적인 for 반복문

in 뒤에는 iterable한 객체(순회 가능한 객체)가 올 수 있다.

```python
num_list = [1, 2, 3, 4, 5]

for num in num_list:
  print(num)

# 결과
# 1
# 2
# 3
# 4
# 5
```

## 2. continue

for 반복문을 수행중 continue를 만나면 다음 반복으로 넘어간다. 즉, continue 아래에 코드가 있더라도 작동하지 않고 다음 순회로 넘어간다.

아래의 코드는 홀수인 숫자만 출력하는 예시이다.

```python
num_list = [1, 2, 3, 4, 5]

for num in num_list:
  # num가 짝수라면 아래의 print를 호출하지 않고 다음 반복 차례로 넘어간다
  if num % 2 == 0:
    continue
  print(num)

# 출력
# 1
# 3
# 5
```

## 3. break

반복을 중지한다. 즉, break를 만나면 해당 반복문을 더 이상 순회하지 않는다. 여기서 주의해야할 점으로는 중첩 반복문일 경우 모든 반복문을 중지하는 것이 아닌 break를 포함하는 반복문 하나만 영향을 받는다.

아래의 코드는 num이 3일때 반복을 중지한다.

```python
num_list = [1, 2, 3, 4, 5]

for num in num_list:
  if num == 3:
    break
  print(num)

# 결과
# 1
# 2
```

### 4. for...else 구문

if조건문 처럼 else 구문을 사용할 수 있다. for 반복문을 문제없이 완료하게 되면 else 구문을 수행한다. 다시말해 break를 만나 도중에 반복이 중지되는 경우 else 구문을 호출되지 않는다. 자주 사용되지는 않으며 for 반복문이 문제없이 제대로 사용되었는지 확인하는 용도로 사용할 수 있다.

```python
num_list = [1, 2, 3, 4, 5]

for num in num_list:
  print(num)
else:
  print('num_list의 모든 원소를 문제없이 출력했습니다')

# 결과
# 1
# 2
# 3
# 4
# 5
# num_list의 모든 원소를 문제없이 출력했습니다
```
