---
title: '[Django]CharField와 TextField에서의 null=True'
categories:
  - django
---

Django에서 필수필드로 만들지 않기 위해 보통은 `null=True, blank=True`로 두는 경우가 많다. 대개의 경우에는 문제가 되지 않지만 문자열 기반 필드인 `CharField / TextField`에 `null=True`를 두는 것을 피하는게 좋다고 한다.

다시 말해, 문자열 기반인 아래의 필드들은 `null=True`를 사용하지 않는 게 권장된다.

- CharField
- TextField
- SlugField
- EmailField
- CommaSeparatedIntegerField
- UUIDField

그 이유는 다음과 같다.

**Django 표준은 빈 값을 빈 문자열로 저장하는 것이며 일관성을 위해서 null값과 빈 값을 빈 문자열을 통해 저장하는 것이다.**

### 결론

자주 사용되는 `CharField / TextField`에 대해서는 `null=True`는 사용하지 말자. 해당 필드를 필수로 만들지 않으려면 `blank=True`를 사용하면된다. 이렇게 설정하면 DB에서는 빈 값이 빈 문자열('')로 설정되어 null과 빈 값을 빈 문자열으로만 판단할 수 있게 된다는 장점을 가질 수 있다.

---

[참고]  
https://docs.djangoproject.com/en/3.0/ref/models/fields/#null  
two scoops of django
