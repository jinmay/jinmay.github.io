---
title: django에서 임시로 static file 서빙하기
tags:
  - django
  - staticfiles
categories:
  - django
date: 2019-03-11 13:47:20
---

# 임시로 static 파일 서빙하기

uwsgi를 이용하여 배포연습을 하던중 static 파일이 제대로 동작하지 않는 현상이 발생했다. **개발서버를 이용하지 않거나 settings.DEBUG=False인 상태에서 배포를 하게 되면 static과 media 파일에 대해서는 별도의 설정이 필요하다.** 개발서버는 이용하지 않지만 임시로 개발서버에서 static file을 서빙하는 것처럼 하기 위한 설정을 알아보자.



#### development 환경에서 static file을 서빙하기 위한 임시 설정

프로젝트의 urls.py에 아래와 같이 추가해주면 된다.

~~~django
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
	# ... url 경로 	
] + static(settings.STATIC_URL, document_root=STATIC_ROOT)
~~~



조금 더 자세하게 기준을 얘기하자면 다음과 같다.

## django의 static file을 다루는 방법

### 개발서버를 쓰고 debug=True

runserver(개발서버)를 사용중이며 settings.DEBUG=True라면 자동으로 static 파일에 대한 rule이 추가된다. 이는 단지 개발의 편의를 위함이다.

### 개발서버를 쓰지 않거나 debug=False

별도로 static 서빙을 설정해야한다. 



 ## static 파일을 서빙하는 방법

1. AWS의 S3와 같은 클라우드 정적 스토리지 서비스 또는 CDN(Contents Delivery Network) 활용
2. Apache / Nginx 웹서버 활용
3. Django의 패키지 활용 - whitenoise 패키지

