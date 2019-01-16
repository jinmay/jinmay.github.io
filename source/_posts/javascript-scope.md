---
title: 키워드 var / let / const와 스코프
tags:
  - javascript
  - es6
  - scope
categories:
  - javascript
  - es6
date: 2019-01-16 11:33:33
---

# scope

**scope란 범위를 의미한다.** 

자바스크립트에서 변수를 선언할 때 사용하는 키워드에는

* **var**
* **let**
* **const**

세 가지가 있다. 

아래 두 개의 키워드 **- let / const -** 는 es6로 넘어오면서 새로 생겼고, **기존에는 var**를 사용하고 있었다. es5에서 변수를 선언하는 방법은 var 키워드를 사용하는 것 뿐이었는데 조심히 다루지 않으면 많은 문제를 발생시키고는 했다.

### var 과 let

##### 함수 스코프

es6 이전에는 블록 단위 스코프가 존재하지 않았기 때문에 발생하는 문제들이 있었다.

~~~javascript
var name = 'global'; // 전역변수

function test() {
  var local = 'local'; // 지역변수  
  for (var i=0; i<10; i++) {
  }
    
  // 변수 i는 함수 test 스코프를 가지기 때문에 아래 코드로 호출가능
  console.log(i); 
};
~~~

for문 밖에서 변수 i에 접근하지 못하게 하려면 let 키워드를 사용하여 변수를 선언해주어야 한다.

~~~javascript
function test() {
  for (let i=0; i>5; i++) {
	console.log(i);  
  }
	console.log(i); // 에러 발생
};

test();
~~~

즉, 블록 스코프를 사용하기 위해서는 let 키워드를 이용해야 한다.

~~~javascript
function test() {
    for (let i=0; i<5; i++) {
        console.log(i);
    }
    if (true) {
        let ifvar = 'ifvar';
        console.log(ifvar);
    }
    console.log(ifvar); // 에러 발생 - if문의 ifvar에 접근할 수 없음
};

test();
~~~



##### 블록 스코프

es6부터 let을 도입하면서 블록 레벨 스코프를 사용할 수 있게 되었다.

~~~javascript
var a = 123;
console.log(a); // 123
{
    a = 456;
}
console.log(a); // 456
~~~

변수 a는 블록 밖에서 키워드  var로 선언되었다. 이 때에 블록 내부의 코드로 인해서 a의 값이 변하게 되고 다시 출력해보면 변경된 값이 출력 되는 것을 알 수 있다. 블록 내부 스코프에는 변수 a는 없지만 스코프 체인에 의해 블록 밖을 검사하게 되고 따라서 전역변수 a의 값을 변경한 것이다.

위 와 같은 현상을 방지하려면 let을 사용해 변수를 선언한다.

~~~javascript
let a = 123;
console.log(a); // 123
{
    let a = 456;
    let b = 12345;
}
console.log(a) // 123
console.log(b) // error 발생
~~~

let 키워드로 선언된 변수를 블록 스코프를 가진다. 따라서 블록 밖에서 값을 출력 하더라도 블록 내부를 들여다 볼 수 없기 때문에 123이 출력되는 것이다. 또한 변수 b도 블록 스코프를 가지므로 외부에서 접근할 수 없기 때문에 에러를 발생시킨 것이다.



### const

const 키워드는 상수를 선언할때 사용한다. 기존의 자바스크립트에는 var를 통하여 상수 / 변수를 모두 선언했지만, es6에는 const라는 키워드가 추가되어 상수를 선언할때 이용할 수 있다.

~~~javascript
function test() {
  const foo = 'foo';
  foo = 'modified foo'; // error 발생 - const로 선언된 foo 변수는 재할당될 수 없다.
  console.log(foo);
};

test();
~~~

그렇다면 상수와 변수 중에서 어느 것을 사용하는 게 좋은 것일까? 대부분의 사람들은 **일반적인 상황에서는 const를 사용하고 값이 변해야만 할때만 let을 사용하길 추천하고 있다.**

만약 const를 이용하여 배열을 선언했다면 이 배열에 값을 추가하고 삭제하는 작업도 불가능한 것일까?

~~~javascript
function test() {
  const list = ['a', 'b', 'c', 'd'];
  list.push('123');
  console.log(list);
}

test(); // ["a", "b", "c", "d", "123"]
~~~

const 키워드를 이용하여 변수를 선언하면 값이 바뀔 수 없다고 했는데 어떻게 문자열 123이 추가된 것일까? **const의 역할에 대해서 정확하게 이해하자면 불변을 의미하는 것은 아니다.** **const는 값이 재할당 되지 못할 뿐 이미 할당된 배열이나 객체에 대해서 값이 변하는 작업을 못하게 하는 것은 아님을 의미한다.** 

##### immutable array 만들기

~~~javascript
const list1 = ['a', 'b', 'c', 'd'];
list2 = [].concat(list, 'haha');
console.log(list1, list2)
~~~

