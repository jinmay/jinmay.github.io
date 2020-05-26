---
title: django-values-and-values_list
---

'[Django]values와 values_list의 차이'

Django ORM 최적화 중 하나로서 필요한 필드의 값만 가져오기 위해 `values()`와 `values_list()`를 사용한다. 각 메소드의 결과는 어떻게 생겼고 차이점이 무엇인지 정리해본다.

### values()

쿼리셋의 값을 딕셔너리 형태로 반환한다.
