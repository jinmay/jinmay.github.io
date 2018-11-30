---
title: 자바스크립트의 반복문 - for
tags:
  - javascript
  - es6
categories:
  - javascript
date: 2018-08-23 17:12:33
---

# 자바스크립트에서 사용되는 for문에 대해서 알아보자

1. for
가장 일반적으로 사용되는 for문이다. 배열의 크기가 가변적이더라도 대응할 수 있다.
~~~javascript
const data = ['a', 'b', 'c', 'd'];
for (let i = 0; i < data.length; i++) {
  console.log(data[i]);
}
~~~

2. forEach
Array가 제공하고 있는 forEach 메소드와 callback 함수를 통해서 배열을 순회할 수 있다. 이 때에 콜백함수의 인자로는 순회하고 있는 요소가 들어가게 된다.
~~~javascript
const data = ['a', 'b', 'c', 'd'];
data.forEach(function(value) {
  console.log(value);
});
~~~

3. for in
파이썬처럼 for in 함수도 있다. 객체의 프로퍼티에 루프를 실행하게 된다. Array에서는 사용하지 않는 걸 추천하며, 그 이유는 다음과 같다. 원래 object를 위해서 만들어졌기 때문이기도 하며 **Array.prototype.getIndex = function() {}** 처럼 prototype에 직접적으로 새로 생성한 프로퍼티가 있을때 의도치 않게 같이 순회하기 때문이다. 또한 배열의 경우 값(value)를 가져오는 게 아니라 인덱스(index)를 가져오게 됨을 주의하자!
~~~javascript
const data = ['a', 'b', 'c', 'd'];
for (let idx in data) {
  console.log(data[idx]); // 인덱스를 가져오기 때문에 이렇게 순회해야 값을 출력한다.
}
~~~

4. for of
es6에서 새로 생긴 반복문이다. for in과는 다르게 컬렉션의 요소를 순회하는 반복문이다. 배열을 순회할때 알맞으며 문자열 각각의 문자들을 반복할때도 사용한다. for in과의 차이점으로는 idx가 아닌 value(값)을 가져오는 것이다.
~~~javascript
const data = ['a', 'b', 'c', 'd'];
for (let value in data) {
  console.log(value);
}

const test_str = "abcdefg";
for (let char in test_str) {
  console.log(char);
}
~~~