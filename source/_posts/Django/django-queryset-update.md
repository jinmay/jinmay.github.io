---
title: '[django]쿼리셋 업데이트하기'
categories:
  - django
---

쿼리셋을 업데이트 하는 방법으로 크게 두 가지가 있을 것 같다. admin이라는 유저가 작성한 게시물의 쿼리셋를 구하고 특정 필드의 값을 업데이트하는 방법에 대해서 정리한다.

## 1. 반복문을 통해 업데이트

일단 admin유저가 작성한 게시물 쿼리셋을 다음과 같이 구한다.

```python
post_list = Post.objects.filter(author='admin')
```

딱히 의미는 없지만 만약 success라는 BooleanField가 있다고 가정하고 이 필드의 값을 True로 모두 변경한다고 할때, 반복문을 통해 작업을 수행할 수 있다.

```python
for post in post_list:
    post.success = True
    post.save()
```

가장 떠올리기 쉬운 방법이라고 생각하는데 몇 가지 단점들이 존재한다.

- 여러번의 save 함수를 호출 => 만약 post_save와 같은 시그널이 존재한다면 여러번 호출되게 됨
- DB의 과부화(반복하는 만큼 쿼리문 생성)
- 느리다

## 2. 쿼리셋의 update 함수 사용

update 함수를 사용하면 단 하나의 쿼리문을 생성하여 빠르고 효율적으로 작업할 수 있다.

```python
post_list = Post.objects.filter(author='admin')
post_list.update(success=True)
```

## 결과

만약 쿼리셋으로부터 특정 필드의 값들을 한 번에 업데이트해야 한다면 반복문을 사용하지 않고 update 함수를 이용하는 것이 좋다. 단 하나만의 쿼리문을 생성하며 save 함수를 호출하지 않는 다는 것에 유의하자.

---

[참고]  
https://m.blog.naver.com/PostView.nhn?blogId=jung_kj&logNo=221002011537&proxyReferer=https%3A%2F%2Fwww.google.com%2F  
http://recordingbetter.com/django/2017/06/07/Django-ORM  
https://tech.peoplefund.co.kr/2017/11/03/django-db-optimization.html
