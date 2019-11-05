---
title: 화살표 함수
tags:
  - javascript
  - es6
categories:
  - javascript
  - es6
date: 2019-01-21 09:52:12
---

# 화살표 표기법 함수

es6에서 새로 등장한 arrow function이다. function이라는 단어와 중괄호 갯수를 줄이려고 했으며 보이는 것 뿐만아니라 this context와 같은 내부적인 중요한 차이도 있다.

##### arrow function

* function 키워드 생략가능
* 매개변수가 하나라면 괄호 생략 가능
* 함수 body가 식(expression) 하나라면 중괄호와  return 생략 가능

```javascript
const f1 = function() {
    return 'hello';
}
const f1 = () => 'hello';

//////////////////////

const f2 = function(nickname) {
    return `Hello~! ${nickname}`;
}
const f2 = (nickname) => `Hello~! ${nickname}`;
const f2 = nickname => `Hello~! ${nickname}`;

//////////////////////

const f3 = function(a, b){
    return a + b;
}
const f3 = (a, b) => a + b;
```

##### arrow function의 this

화살표 함수가 나오기 이전의 this에 대해서 먼저 살펴보자. 

```javascript
const obj = {
    register() {
        setTimeout(function(){
            console.log(this === window) // true
            this.pprint();
        }, 200)
    },
    pprint() {
        console.log('print @-hello world-@');
    }
};

obj.register(); // TypeError: this.pprint is not a function
```

에러가 발생했다. **그 이유는 this가 obj에 걸린 것이 아닌 window에 묶여있기 때문이다(this === window의 콘솔 결과를 보면 알 수 있다). 이 때 bind(this)를 사용하거나 화살표 함수를 통해서 obj에 lexical하게 this를 묶을 수 있다.**

```javascript
// .bind()
const obj = {
    register() {
        setTimeout(function(){
            this.pprint();
        }.bind(this), 200)
    },
    pprint() {
        console.log('print @-hello world-@');
    }
}

obj.register(); // print @-hello world-@

// 화살표 함수
const obj = {
    register() {
        setTimeout(() => {
            this.pprint();
        }, 100)
    },
    pprint() {
        console.log('print @-hello world-@');
    }
}

obj.register(); // print @-hello world-@
```

