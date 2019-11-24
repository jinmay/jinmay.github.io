---
title: [파이썬]private 변수(private 네임 맹글링)
categories:
  - python
date: 2019-11-24 17:35:38
tags:
---

파이썬은 자바와 다르게 접근제어자가 없다. 사실 클래스 정의 외부에서 접근할 수 있도록 만드는 방법이 있긴한데 접근제어자처럼 완벽하게 통제할 수는 없다.

```python
class Duck():

  def __init__(self, name):
    self.__name = name

a = Duck('son')
print(a.__name)
# 'Duck' object has no attribute '__name'
```

더블 언더스코어를 붙여서 변수를 만들면 된다. 이때에 **\_\_name을 직접 접근할 수 없다!!** 완벽한 private은 아니지만 외부 코드에서 발견할 수 없도록 이름을 맹글링(뭉게기 - mangling) 했다고 보면 된다. 완벽한 private 변수라고 말할 수 없는 이유는 네임 스페이스를 확인해보면 알 수 있을 것이다.

```python
print(dir(a))
# a._Duck__name
# 기타 등등
```

아예 접근 불가능한 것이 아니라, **접근할 수 있는 변수의 이름이 달라지는 것이라고 이해할 수 있다!!**

---

[참고]  
패스트 캠퍼스 강의
