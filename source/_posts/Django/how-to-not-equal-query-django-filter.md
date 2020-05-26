---
title: '[Django]not 연산으로 조건 부정하기'
categories:
  - django
---

Django에서 not 연산을 사용하기 위해서는 다음과 같은 연산자를 사용한다.

1. not
2. ~

if문과 같은 조건절이라면 not을 사용할 수 있지만 `queryset.filter()`나 `queryset.get()`에서는 어떻게 해야할까?

Django에서 not 연산을 사용하기 위해서는 아래의 두 메소드를 사용하면 된다.

1. exclude()
2. filter(~Q(조건))

`exclude()`의 경우 queryset에서는 기본적으로 사용가능하며, `~Q(조건)`의 형태로 사용하려면 Q를 임포트해야한다.  
(`from django.db.models import Q` 임포트 문을 통해 가져올 수 있다.)

### exclude()

기본적으로 queryset의 객체라면 not 연산을 사용하기 위해서 exclude() 메소드를 사용할 수 있다.

```python
projects = Project.objects.exclude(title__startswith='hello')
```

### ~Q(condition)

Q를 임포트하여 `~` 기호를 사용해 not 연산을 할 수 있다.

```python
from django.db.models import Q

projects = Projects.objects.filter(~Q(title__startswith='hello'))
```

---

[참고]  
https://django-orm-cookbook-ko.readthedocs.io/en/latest/notequal_query.html
