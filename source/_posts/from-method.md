---
title: 진짜 배열을 만들기위한 from 메소드
tags:
  - javascript
  - es6
categories:
  - javascript
date: 2018-08-23 17:58:57
---

# Javascript의 from 메소드

### 진짜 배열로 바꾸기 위해 from을 사용한다!!!

일단 실습을 위해 배열의 각 요소에 느낌표(!)를 더하는 함수를 만들어보자
~~~javascript
function addMark() {
  let newData = [];

  for (let i=0; i<arguments.length; i++) {
    newData.push(arguments[i] + "i");
  }

  console.log(newData);
}

addMark(1, 2, 3, 4, 5);
~~~
이 예제코드에는 처음보는 **arguments**라는 객체가 있다. arguments는 진짜 배열은 아니지만 마치 배열인 것 처럼 쓰인다. 위의 코드에서 addMark라는 함수를 생성할때 몇개의 인자를 필요로하는지 명시하지 않았다. 하지만 코드는 제대로 작동하게 되는데 **arguments 객체가 내부적으로 특수한 변수를 만들어 사용했기 때문에 가변인자에 대해서 대응할 수 있었던 것이다.**

arguments 객체를 배열처럼 사용할 수 있다면
~~~javascript
let newData = arguments.map(function(value) {
  return value + "!";
});
~~~
위와 같은 코드도 가능하다. **하지만 실제로 위와 같이 바꾼다면 에러가 나게된다.**

그 이유는 무엇일까?

**arguments는 마치 배열처럼 행동할 뿐 진짜 배열은 아니기 때문이다.** 그래서 진짜 배열로 바꾸어 주면 map 메소드가 제대로 실행되게 되는데 이 때에 **from** 메소드를 이용한다.
~~~javascript
let newArray = Array.from(arguments);
let newData = newArray.map(function(value) {
  return value + "!";
});
~~~