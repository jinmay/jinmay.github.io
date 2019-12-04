---
title: Array와 반복문
tags:
  - javascript
  - es6
categories:
  - javascript
date: 2019-01-17 09:40:56
---

# 배열(Array)

자바스크립트에서 배열을 반복하는 방법엔 여러가지가 있다.

##### forEach

```javascript
let data = [1, 2, undefined, ""];
data.forEach(function(value) {
  console.log(value);
});

// 1
// 2
// undefined
// ""
```

##### for ... in

객체를 순회할때 사용한다. 과거에는 객체 뿐만아니라 array에도 사용을 했으며 **프로토타입에 걸려있는 함수 또는 변수도 출력하기 때문에 주의를 요구한다.** 프로토타입에 추가적으로 정의된 게 없다면 문제없지만 프로토타입에 아무것도 없다는 것을 보장하기 힘들기 때문에 배열에는 사용하지 않는 편이다. 만약 **배열에 반복문을 사용하고 싶다면 es6에서 새로 추가된 for ... of 반복문을 사용하는 것이 좋다.**

```javascript
let data = {
    'a': 1, 
    'b': 2,
    'c': 3
};

for (let key in data) {
    console.log(key, data[key]);
}
```

**for ... in 구문은 객체의 key를 사용한다.** 따라서 객체의 key에 따른 value를 얻기 위해서는 data[key]의 형식으로 적어주어야 한다. 앞서 말한것처럼 객체가 아닌 Array에 for ... in 구문을 사용하게 되면 생각지 못했던 값이 출력될 수 있다.

```javascript
let data = ['a', 'b', 'd'];

Array.prototype.testFunc = function() {};
for (let key in data) {
    console.log(data[key]);
}

// 출력 결과
// 'a'
// 'b'
// 'd'
// function(){}
```

##### for ... of

es6에서 추가된 반복문이다. 컬렉션의 요소에 루프를 실행하기 위해서 사용한다.

```javascript
let data = [1, 2, undefined, ""];

for (let value of data) {
  console.log(value);
};
```

**for ... in 반복문과는 다르게 컬렉션의 값을 직접 사용할 수 있다.** 뿐만아니라 문자열에도 사용할 수 있다.

```javascript
let data = "hello world";

for (let v of data) {
  console.log(v);
}
```



## spread operator

파이썬의 **튜플 패킹 / 언패킹**과 비슷하게 작동한다.

```javascript
let first = ['snake', 'pig', 'hello'];
let newData = [...first];

console.log(first); // ['snake', 'pig', 'hello']
console.log(newData); // ['snake', 'pig', 'hello']

// 결과는 같지만 서로 다른 배열임
// 기존의 참조를 끊고 새로 연결함
console.log(first === newData); 
```

##### 삽입하기

```javascript
let test = ['a', 'b', 'c', 'd'];
let data = [1, 2, ...test, 3, 4];

console.log(data); // [1, 2, "a", "b", "c", "d", 3, 4]
```

##### 인자로 풀어넣기

```javascript
function mysum(a, b, c) {
  return a + b + c;  
};

let data = [1, 2, 3];
console.log(mysum(...data));
```

##### 가변인자

```javascript
function addMark() {
    
    // 첫 번재 방법 - for 반복문과 arguments 객체 이용
	let result = [];
    for (let idx=0; idx<arguments.length; idx++) {
        result.push(arguments[idx] + "!!");
    }
    
    // 두 번재 방법 - from과 map 그리고 arguments 객체 이용
    let newArray = Array.from(arguments);
	let result = newArray.map(function(value) {
        return value + "!!";
    });
    
    console.log(result);
};

addMark(1, 2, 3, 4);
```

**arguments 객체는 모든 함수 내에서 이용 가능한 지역변수이다.** 보통은 함수에 인자가 몇 개 들어올 지 예상할 수 없을때 사용한다. **arguments 객체는 Array처럼 생기긴 했지만 length 메소드 빼고는 어떤 Array 속성도 가지고 있지 않다고 한다.** 

```javascript
let result = arguments.map(function(value) {
   return value + "!"; 
});
```

만약 두 번재 방법에서 위와 같이 사용했다면 에러가 발생할 것이다. **왜냐하면 arguments 객체는 배열처럼 보일뿐 실제로 배열은 아니기 때문이다.**