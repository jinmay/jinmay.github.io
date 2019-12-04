---
title: Map과 WeakMap
tags:
  - javascript
  - es6
categories:
  - javascript
date: 2019-01-20 22:49:04
---

# Map과 WeakMap

맵은 Key와 Value를 연결한다는 점에서 객체와 유사하다. 하지만 객체를 사용할때 발생하는 문제점들이 있는데 아래와 같다.

* 프로토타입 체인으로 인해 의도하지 않은 연결이 생김
* 객체는 프로퍼티의 순서를 보장하지 않음
* 객체 안에 연결된 키와 값이 몇 개 인지 알기 어려움
* 객체의 키는 반드시 문자열 또는 Symbol이어야 하므로 객체를 키로 사용할 수 없음

Map은 위에서 나열한 객체의 단점들을 해결했다고 한다.

## Map

```javascript
let m = new Map();

m.set('data', [1, 2, 3]);
m.set('data2', 1);

console.log(m.get('data')); // [1, 2, 3]
```

Set과 비슷하게 작동한다. set()을 통해 맵에 데이터를 추가할 수 있고 get()으로 가져올 수 있다.

## WeakMap

WeakSet과 비슷하게 동작한다. 위크셋과 마친가지로 참조 타입의 객체만 포함할 수 있다. 다음은 위크셋의 키로 myfunc라는 함수를 사용하고 반복문을 통해 값을 1부터 10까지 더한 후 저장하는 예제이다.

```javascript
let wm = new WeakMap();
let myfunc = function(){};

wm.set(myfunc, 0);

sum = 0;
for (let i=1;i<=10;i++){
  sum += i;
}
wm.set(myfunc, sum);
console.log(wm); // WeakMap { myfunc() -> 55}
```

두 번째로 위크맵을 통해 인스턴스 변수를 보호하는 예제이다.

```javascript
function Area(height, width){
    this.height = height;
    this.width = width;
}

Area.prototype.getArea = function(){
    return this.height * this.width;
}

let myarea = new Area(10, 20);
console.log(myarea.getArea()); // 200
console.log(myarea.height); // 10
```

맨 마지막줄을 살펴보자. 아마도 height 변수의 값이 출력 될 것이다. 이처럼 외부에서 클래스의 내부 인스턴스 변수를 직접 접근하는 것은  좋지 않다. WeakMap을 이용하여 외부로부터의 접근을 막아보자.

```javascript
const wm = new WeakMap();

function Area(height, width){
    wm.set(this, { height, width });
}

Area.prototype.getArea = function(){
	const { height, width } = wm.get(this);
    return height * width;
};

let myarea = new Area(10, 20);
console.log(myarea.getArea()); // 200
console.log(myarea.height); // undefined
```

WeakMap과 destructuring을 통해서 코드를 작성했다. 위크맵이 전역적으로 사용되는 단점이 있지만 외부 접근을 막는 데에는 성공한 것을 볼 수 있다!