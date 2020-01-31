---
title: django-sum-aggregation
---

'[Django]Django ORM을 이용해 테이블의 컬럼의 합 구하기'

보통 ORM을 통해 작업을 하다보면 작업의 단위는 하나의 row(행, 튜플)일 것이다. Django에서 제공하는 Aggregation을 이용하면 CRUD 작업 이외의 것들도 수행할 수 있는데 그 중에서 필드의 합을 구하는 Sum에 대해서 정리해본다.

### 모델

아래와 같은 Item 모델이 있다고 가정하고 시작하자. 1, 2와 같이 단순 숫자로된 카테고리와 상품의 가격 필드를 가지고 있다.

```python
class Item(models.Model):
    category = models.IntegerField()
    price = models.IntegerField()
```

### Aggregate

복수 개의 item이 있고 카테고리 번호가 3번인 상품들에 한해서 price의 합을 구하고 싶으면 아래와 같이 할 수 있다. Sum 함수를 임포트 해야하고 **인자로써 합계를 구하려는 필드명을 적어주어야 한다.**

```python
# Sum 함수를 임포트 해주어야 한다
from django.db.models import Sum

total_price = Item.objects.filter(category=3).aggregate(Sum('price'))
```

aggregate의 결과를 total_price라는 변수에 저장을 했다. aggregate의 결과 값은 dict형으로 반환되며 아래와 같이 사용하면 된다.

```python
# 기본형
# obj[<field명__aggregate명>]

total_price['price__sum']
```

---

[참고]  
https://fun25.co.kr/blog/python-django-orm-aggregate-sum/?category=002  
https://docs.djangoproject.com/en/3.0/topics/db/aggregation/  
http://raccoonyy.github.io/django-annotate-and-aggregate-like-as-excel/  
https://wayhome25.github.io/django/2017/09/02/django-queryset-aggregate-coalesce/
