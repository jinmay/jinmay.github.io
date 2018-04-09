---
title: css 플렉스 박스 기본
tags:
  - css
  - flexbox
categories:
  - archive
date: 2018-04-09 16:06:35
---

09년에 나왔다고는 하지만 17년이 되도록 존재조차 몰랐던 Flexbox에 대해서 알아보자. 플렉스박스는 CSS의 display 속성을 flex로 줌으로써 html의 레이아웃을 표현하는 방식이다. 아직까지는 인터넷 익스플로러와 같은 구버전 브라우저의 지원률이 미흡하지만 웹 페이지의 레이아웃을 구현하는 데 있어서 기존에 사용했던것 보다 더 간편한 듯 싶다.

사용법은 아래와 같다. 

~~~css
display: flex;
~~~

**첫째로 flexbox 레이아웃을 구성할 때는 부모 컨테이너에 flex를 주어야 한다.**

~~~css
.container {
    display: flex;
}
~~~

~~~html
<div class="container">
    <div class="box"></div>
    <div class="box"></div>
    <div class="box"></div>
</div>
~~~

display: flex 로 인해서 div.container 는 flex container가 되었으며 컨테이너 안의 div.box 들은 flex 항목이 된 것이다.(.container는 플렉스 서식 환경을 만들게 되고 .box는 플렉스 항목이 된다.)


## Flex container
플렉스 항목들을 감싸고 있는 플렉스 컨테이터네 적용시키는 속성

### 플렉스박스 방향
플렉스 컨테이너를 만드는 경우 컨테이너 내 하위 태그의 방향을 지정할 수 있다. 속성명은 **flex-direction**이면 웹 페이지에서 기본값으로 **row**로 설정되어있다.(React Native에서 사용하는 flexbox에서는 기본 방향이 모바일 어플리케이션처럼 column으로 되어있다.) 전체 플렉스 항목들의 흐름 방향을 제어하기 때문에 플렉스 항목이 아닌 **플렉스 컨테이너에서 정의**해주어야 한다.
  * 속성 : flex-direction
  * 속성값
    * row: flex-direction의 기본값이며 왼쪽에서 오른쪽으로 나열한다
    * row-reverse - 오른쪽에서 왼쪽으로 나열한다
    * column - 플렉스 항목들을 수직으로 위에서 아래쪽을 향해 나열한다
    * column-reverse - 항목들을 수직으로 아래에서 위로 나열한다


### 플렉스 항목 줄바꿈
플렉스 항목들이 컨테이너의 크기를 초과할때 줄바꿈에 대한 속성이다. 즉 플렉스 항목을 감쌀 것인지 아닌지를 지정한다.
  * 속성 : flex-wrap
  * 속성값
    * nowrap : 기본값이며 항목들이 크기를 초과하더라도 줄바꿈을 하지 않는다
    * wrap : 플렉스 항목들이 컨테이너 안에서 여러줄로 줄 바꿈이 가능하도록 한다
    * wrap-reverse : wrap으로 생기는 줄바꿈의 정렬 방향을 반대로 한다


### 플렉스 항목들의 진행 방향과 줄 바꿈
flex-direction과 flex-wrap의 단축 속성이다.
  * 속성 : flex-flow
    * 초기값은 row nowrap 
  * 속성값
    * diredction wrap 순으로 작성한다


### 정렬
**기본 축(주축) 정렬**과 **교차 축 정렬**을 수행하는 속성이 있다. 
1. justify-content - 기본 축 정렬
2. align-items - 교차 축 정렬

* 속성값
  1. flex-start
  2. flex-end
  3. center
  4. space-between
  5. space-around


### 참고
멋사 세션을 진행하던 도중 본인도 헷갈렸던 것이 있었는데 **align-content** 이다.

align-items와 마찬가지로 주 축에 대한 교차 축 정렬을 하게 되는데 이 속성은 많은 아이템이 한 줄 이상 넘어가는 경우에 사용한다고 한다. 만약에 플렉스 컨테이너 내의 플렉스 항목들이 한 줄을 넘어가지 않는다면 align-content 속성을 제대로 작동하지 않는다는 의미이다. align-items와 justify-content 속성이 가지는 특성들을 모두 포함하고 있기는 하지만 적용되는 상황이 다른만큼 각별한 주의가 필요하다.
  * 속성 : align-content
  * 속성값
    * 초기값은 stretch
    * flex-start
    * flex-end
    * center
    * space-between
    * space-around
    * stretch



## Flex item
플렉스 항목들이 가지는 속성

### order
컨테이너 안에서 항목들이 나열되는 순서를 의미한다. 숫자가 작을수록 기준 축에 대해서 시작하는 항목이며 높을수록 반대쪽에 위치하게 된다. CSS의 z-index와 비슷하며 음수사용이 가능하다. 

order 속성의 정의되면 HTML이나 CSS에서 구문을 어떤 순서에 따라 작성했는지에 상관없이 order에 선언된 값에 의해서만 순서가 정해진다. 


### flex
flex-grow / flex-shrink / flex-basis의 축약속성이다. 기본값은 0 1 auto이다.

1. flex-grow : 플렉스 항목의 팽창을 제어함
 * 음수사용 불가
 * 초기값은 0

2. flex-shrink : 플렉스 항목의 수축을 제어함
 * 음수사용 불가
 * 초기값은 1

3. flex-basis : 플렉스 항목의 기본 크기를 설정함
 * 초기값은 auto

4. flex : 위의 세 가지 속성을 한번에 지정할 수 있는 축약속성
 * 초기값 : 0 1 auto



# 정리
1. flex container에만 적용 가능한 속성
~~~css
.flex_container {
  display: flex;                  /* flex container 선언 */
  flex-direction: row;            /* 정렬 방향*/
  flex-wrap: nowrap;              /* 줄 바꿈 */
  justify-content: flex-start;    /* 기본 축(진행 축/주 축) 정렬 */
  align-items: stretch;           /* 교차 축 정렬 */
  align-content: stretch;         /* 여려줄 교차축 정렬 */
}
~~~

2. flex items에만 적용 가능한 속성
~~~css
.flex-items {
  flex-grow: 0;               /* 팽창 지수 */
  flex-shrink: 1;             /* 수축 지수 */
  flex-basis: auto;           /* 기준 크기 */
  align-self: auto;           /* 독립적 교차축 정렬 */
  order: 0;                   /* 배치 순서 */
}
~~~


[참고]
<http://webclub.tistory.com/259>
<http://webclub.tistory.com/260?category=541913>
<http://www.beautifulcss.com/archives/2812>
<https://www.slideshare.net/wsconf/css-flex-you-must-konw-wsconfseoul2017-vol2>
<http://webdir.tistory.com/349>