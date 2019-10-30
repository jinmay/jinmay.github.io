---
title: Django에서 ajax post 요청시 csrf token 문제 해결하기
tags:
  - django
  - django csrf
  - ajax
  - django ajax
categories:
  - django
date: 2019-04-09 17:38:37
---

# Django에서 ajax 사용할때 발생하는 csrf 문제
장고에서 ajax를 조금씩 섞어서 사용하다보면 POST 요청을 보낼때 문제가 발생하게 된다. POST 요청에서는 보안상의 문제로 csrf token을 필요로하게 되는게 장고의 기본기능을 사용하지 않고 ajax를 통해서 요청하기 때문에 생기는 현상이다. 답은 의외로 간단한데 장고 공식페이지를 보면 두 가지를 얘기해주고 있다.

> 1. @csrf_exempt 데코레이터 사용
> 2. ajax post 요청을 발생시킬때 csrf token 생성 및 추가 

하나씩 살펴보자.

1. @csrf_exempt 데코레이터 사용

데코레이터를 이용해서 해결하는 방법이다. 장고 프로젝트의 설정을 보면 아래와 같이
~~~python
'django.middleware.csrf.CsrfViewMiddleware',
~~~
csrf에 관련한 미들웨어가 있는 것을 볼 수 있다. 이 미들웨어 때문에 csrf에 대한 보안을 강화할 수 있는 건데 @csrf_exempt 데코레이터를 사용하게되면 csrf 보호 기능을 받지않아 csrf token을 사용하지 않아도 되게 된다. 
~~~python
from django.http import HttpResponse
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def my_view(request):
    return HttpResponse('Hello world')
~~~
즉 my_view 함수를 post 요청으로 호출할때마다 미들웨어를 통해 csrf token을 검사받아야 하지만 그러지 않아도 된다는 것이며 편리하지만 보안적인 측면을 포기하는 것이기 때문에 권장하지 않는 방법이다.

2. js를 통해 csrf token 생성 및 주입

장고 공식홈페이지를 보면 자바스크립트로 ajax post 요청시에 csrf token을 만들고 삽입하는 코드가 나와있다. ~~그래서 그냥 쓰면된다~~
~~~javascript
function getCookie(name) {
    var cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        var cookies = document.cookie.split(';');
        for (var i = 0; i < cookies.length; i++) {
            var cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}

var csrftoken = getCookie('csrftoken');

function csrfSafeMethod(method) {
    // these HTTP methods do not require CSRF protection
    return (/^(GET|HEAD|OPTIONS|TRACE)$/.test(method));
}
$.ajaxSetup({
    beforeSend: function(xhr, settings) {
        if (!csrfSafeMethod(settings.type) && !this.crossDomain) {
            xhr.setRequestHeader("X-CSRFToken", csrftoken);
        }
    }
});
~~~
코드는 위와 같고 평소 js 사용하듯이 템플릿에 추가해주면 된다. HTML의 body에 그냥 추가하지 말고 장고의 static 기능을 추가해서 관리하면 더 좋을 것 같다.

<hr>
<참고> 

https://docs.djangoproject.com/en/2.2/ref/csrf/