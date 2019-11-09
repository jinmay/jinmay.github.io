---
title: 장고 쿼리셋의 select_related
categories:
  - django
date: 2019-11-09 23:37:05
tags:
---

> debug_toolbar를 설치해서 SQL 항목을 보기 전까지 select_related와 prefetch_related는 아직은 필요하지 않은 것들이라고 생각했다. 데이터가 많지 않아 드라마틱한 속도는 볼 수 없었지만 duplicated를 보지 않는 것만으로도 효과는 충분히 보았다고 생각한다.

장고 ORM을 사용할 수 있다는 것은 정말 큰 장점이 되는 것 같다. 물론 이 ORM이 어떻게 동작하는 지 이해하고 쓴다면 더할 나위 없이 좋겠지만 차차 알아보기로 하고 지금은 select_related에 대해서 정리해 보려고 한다.

일단 아래와 같은 모델이 있다고 가정하고 시작해보자.

```python
# 게시글
class Post(models.Model):
    title = models.CharField(max_length=100)
    author = models.ForeignKey('user', on_delete=models.CASCADE)

# 글쓴이
class User(models.Model):
    name = models.CharField(max_length=10)
```

## select_related

모든 게시글에 대해서 글쓴이를 순회하는 출력하는 HTML 코드가 있다면 아래와 같을 것이다.

```django
{% for post in post_list %}
  <p>글쓴이 : {{ post.author }}</p>
{% endfor %}
```

정말 흔하게 작성하는 코드이기 때문에 큰 의문이 들지 않을 수도 있지만, 장고의 QuerySet의 특성 한 가지를 이해한다면 비효율적으로 실행되는 것을 이해할 수 있게된다.

여기서 필요한 쿼리셋의 특성 한 가지는

- QuerySet은 기본적으로 지연평가(Lazy Evaluation)를 한다는 것

이다.

### 지연평가란?

**파이썬 코드 상에서 QuerySet을 만드는 작업은 DB에 아무런 작업을 수행하지 않는 다는 것이다.** 즉, all() / filter() 과 같이 QuerySet을 생성하는 작업들은 DB에 접근하는 작업이 아니며 실제 값을 뽑는 - 연산 - 과정이 실행되기 전까지 DB query가 일어나지 않는다.

이러한 지연평가로 인해서 views.py에서 쿼리셋을 생성하는 작업들을 반복해서 수행해도 DB 호출을 최소화 할 수 있다는 장점을 가질 수 있다. 하지만 템플릿 상에서 반복문을 통해 외래키를 참조하는 쿼리셋을 평가하는 작업을 반복하게 된다면 SQL문의 중복이 발생할 수 있다는 단점도 있다.

다시 코드로 돌아와서..  
쿼리셋의 지연평가를 이해하면

```django
{% for post in post_list %}
  <p>글쓴이 : {{ post.author }}</p>
{% endfor %}
```

**{{ post.author }}** 부분이 모든 post 객체에 대해서 실행되기 때문에 post 객체의 개수 만큼 쿼리가 중복 발생하는 것을 파악할 수 있다.

문제를 해결하기 위해선 views.py 에서 post_list를 구할때 **select_related**를 사용하면 된다.

```python
post_list = Post.object.select_related('user').all()
```

post 객체를 만들때 외래키 관계에 있는 객체를 한 번에 다 가져온다. HTML 상에서 지연평가를 하지 않고 DB상에서 Inner join을 통해 한 번의 접근만으로 필요한 정보를 가져오게 되는 셈이다.

지금까지 select_related에 대해서 의식의 흐름대로 정리해 보았다. 이제 막 찾아보고 처음으로 사용했던 지라 제대로 소화하지 못한 것 같아서 prefetch_related 정리할 때 다시 해야겠다.


---
[참고]
<https://wayhome25.github.io/django/2017/06/20/selected_related_prefetch_related/>
<https://tech.peoplefund.co.kr/2017/11/03/django-db-optimization.html>
<https://blog.leop0ld.org/posts/database-access-optimization/>
