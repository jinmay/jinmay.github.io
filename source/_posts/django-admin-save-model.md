---
title: 어드민 패널의 save_model 오버라이드
tags:
  - django
  - django admin
categories:
  - django
date: 2019-08-01 17:52:51
---

# admin app의 save_model 메소드

어드민 패널의 save_model 메소드는 모델을 저장하는 작업의 전, 후에 영향을 미칠 수 있다. ModelForm을 사용중이라면 admin.py에 form 클래스는 따로 명시해주지 않았을때 기본적으로 생성해서 사용하지만, form 클래스를 만들어서 어드민 패널에서만 사용할 수 있도록 할 수 있다.

나의 경우 이미 앱에서 사용 중인 모델 폼이 있었는데, 이 모델 폼을 어드민 패널에서 사용하기에는 조금 안맞는 부분이 있어서 어드민에서만 사용할 폼을 생성해서 admin.py에 명시하여 수정하는 방법을 사용했다.

어드민에서 모델 인스턴스를 저장할때 save_model 메소드를 오버라이딩해서 로직을 수정할 수 있다.

> > ModelAdmin.save_model(request, obj, form, change)

네 개의 인자를 받는다.

- request
- 모델 인스턴스(model instance)
- 모델 폼 인스턴스(modelform instance)
- 생성인지 수정인지 나누는 부울 값(True / False)

```python
from django.contrib import admin

class ArticleAdmin(admin.ModelAdmin):
    def save_model(self, request, obj, form, change):
        obj.user = request.user
        super().save_model(request, obj, form, change)
```

장고 공식페이지의 예제이다. Article 모델의 어드민 관련 코드이며 save_model 메소드를 오버라이드 하고 있다.

모델 코드를 보지 않고서라도 Article 모델에 user라는 일대일 관계를 가진 필드가 있음을 알 수 있다. 현재 어드민에 로그인한 유저를 Article 모델 인스턴스의 user 필드에 저장을 하기 위한 코드인 것 같다.
