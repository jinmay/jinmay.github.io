---
title: '[Django]values와 values_list의 차이'
categories:
  - django
---

Django ORM 최적화 중 하나로서 필요한 필드의 값만 가져오기 위해 `values()`와 `values_list()`를 사용한다. 각 메소드의 결과는 어떻게 생겼고 차이점이 무엇인지 정리해본다.

### values()

쿼리셋의 값을 딕셔너리 형태로 반환한다. queryset에 대해서 사용하기 때문에 순서는 그닥 상관 없는 것 같다.

```python
Post.objects.values().filter(id__lt=8)
# <QuerySet [{'id': 5, 'title': 'post #1'}, {'id': 6, 'title': 'title #1'}, {'id': 7, 'title': 'title #2'}]>

Post.objects.filter(id__lt=8).values()
# <QuerySet [{'id': 5, 'title': 'post #1'}, {'id': 6, 'title': 'title #1'}, {'id': 7, 'title': 'title #2'}]>
```

만약 아래와 같이 `values()` 메소드에 인자로서 필드명을 넣으면 `필드: 값`의 형태로 가져올 수 있다.

```python
Post.objects.filter(id__lt=8).values('title')
# <QuerySet [{'title': 'post #1'}, {'title': 'title #1'}, {'title': 'title #2'}]>
```

### values_list()

쿼리셋의 값을 튜플 형태로 반환한다. 

```python
Post.objects.filter(id__lt=8).values_list()
# <QuerySet [(5, 'post #1'), (6, 'title #1'), (7, 'title #2')]>
```

마찬가지로 `values_list()`에 인자로서 필드를 넣으면 해당 필드의 값만 튜플로 반환한다.

```python
Post.objects.filter(id__lt=8).values_list('title')
# <QuerySet [('post #1',), ('title #1',), ('title #2',)]>
```

`values_list()`에는 필드명 이외에 `flat`이라는 인자를 사용할 수 있다. `flat`은 Boolean 타입이며 기본값은 `False`이다. `flat`의 역할은 튜플이 아닌 리스트로 필드의 값을 반환하는 것이다. 주의사항으로 `flat` 인자는 필드가 여러개일때 사용할 수 없다.

```python
Post.objects.filter(id__lt=8).values_list(flat=True)
# <QuerySet [5, 6, 7]>

Post.objects.filter(id__lt=8).values_list('title', flat=True)
# <QuerySet ['post #1', 'title #1', 'title #2']>
```

### 정리

ORM을 사용하면 매우 편리하게 DB를 사용할 수 있어서 좋지만 때로는 비합리적으로 동작할때가 있어 대용량 데이터를 다룰때 문제가 될 수 있다. `values()`와 `values_list()`는 ORM 쿼리 최적화의 방법 중 하나로서 필요한 필드의 값만 가져올 수 있고, 따라서 DB에 부하를 줄일 수 있다는 점을 알고 있자.