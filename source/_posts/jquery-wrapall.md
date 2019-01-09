---
title: wrapAll 메소드
tags:
  - jquery
categories:
  - javascript
  - jquery
date: 2019-01-09 18:34:00
---
# .wrapAll() 메소드

### 형태

~~~javascript
.wrapAll(wrappingElement)
~~~

### 설명

선택된 HTML 구조(태그)들을 괄호안의 wrappingElement로 감싼다.

### 예시

~~~html
<div class="container">
  <div class="inner">Hello</div>
  <div class="inner">Goodbye</div>
</div>
~~~

위 와 같이 HTML이 있을 때, 클래스가 .inner인 태그들을 감싼다

~~~javascript
$('.inner').wrapAll("<div class='new' />")
~~~

결국 아래와 같은 HTML DOM을 형성하게 된다.

~~~html
<div class="container">
  <div class="new">
    <div class="inner">Hello</div>
    <div class="inner">Goodbye</div>
  </div>
</div>
~~~

