---
title: Set과 WeakSet
tags:
  - javascript
  - es6
categories:
  - javascript
  - es6
date: 2019-01-18 10:14:45
---

# Set과 WeakSet

배열과 비슷하게 생긴 것중에 **Set과 WeakSet 이라는 게 있다.** 

## Set

**일반적인 배열과는 다르게 중복을 허용하지 않고 유일한 값만을 저장한다.** 이미 존재하는 지 확인할 때 유용하다.

~~~javascript
let mySet = Set();
console.log(toString.call(mySet)); // [object Set]
~~~

형태는 배열과 똑같이 생겼지만 Set 안에 중복된 값이 있을 경우 하나만 저장된다. 메소드를 통해 값을 넣고 뺄 수 있다.

~~~javascript
let mySet = Set();

mySet.add('abc');
mySet.add('Hello');
mySet.add('abc');
mySet.add('abc');

mySet.forEach(function(value){
   console.log(value); 
});

// 'abc'
// 'Hello'
~~~

"abc"라는 문자열을 총 세 번 입력했지만 Set의 특성상 중복을 제거해 주기 때문에 한 번밖에 사용되지 않았다.

~~~javascript
mySet.delete('abc'); // ['Hello']
~~~

삭제를 위해서 delete 메소드를 사용했다.

~~~javascript
if (mySet.has('Hello')) {
    console.log('True');
} else {
    console.log('False');
}
~~~

mySet이라는 Set에 "Hello" 문자열이 있는지 판단했다. 만약 해당 문자열이 있다면 "True"를 아니라면 "False"를 출력한다. **정리하자면 Set은 중복을 허용하지 않는 데이터 집합이라고 할 수 있다.**

## WeakSet

**참조를 가지고 있는 객체만 저장이 가능하다.** **그리고 이 객체들은 가비지 콜렉션의 대상이 된다.**

~~~javascript
let data = [1, 2, 3, 4];
let ws = new WeakSet();

ws.add(data);
ws.add(123); // error
ws.add('haha'); // error
ws.add(function(){});

console.log(ws); // WeakSet{[1, 2, 3, 4], ()}
~~~

숫자 123과 문자열 'haha'는 참조를 가지고 있는 객체 타입이 아닌 기본형(primitive type) 타입 이기 때문에 WeakSet에 저장하려고 시도하면 에러가 발생한다. 그러나 함수는 참조 타입이므로 문제없이 WeakSet에 저장할 수 있다. 

그렇다면 WeakSet에 저장되어 있는 객체들이 **가비지 콜렉션의 대상**이 된다는 말을 무엇일까?

~~~javascript
let data = [1, 2, 3, 4];
let data2 = ['a', 'b', 'c'];
let obj = { data, data2 };
let ws = new WeakSet();

ws.add(data);
ws.add(data2);
ws.add(obj);

console.log(ws); // WeakSet([1, 2, 3, 4], ['a', 'b', 'c'], { data, data2 })
~~~

위의 코드를 보면 위크셋 ws에는 두 개의 배열과 하나의 객체가 들어가있다. 위크셋에 저장되어있는 변수 data에 null을 대입해보자.

~~~javascript
data = null;

console.log(ws); // WeakSet([1, 2, 3, 4], ['a', 'b', 'c'], { data, data2 })
~~~

콘솔에 출력된 결과는 null을 대입하기 이전과 다를 것이 없어보인다. 하지만 has 메소드를 통해서 확인해보면 정말 유효한지 아닌지 알 수 있다.

~~~javascript
console.log(ws.has(data), ws.has(data2)); // false, true
~~~

data는 null을 대입해 주었기 때문에 더 이상 유효하지 않으며 data2는 그대로 배열을 가지고 있어서 true를 출력한다. 

