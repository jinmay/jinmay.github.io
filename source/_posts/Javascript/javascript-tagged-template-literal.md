---
title: 템플릿 태그 리터럴
tags:
  - javascript
  - es6
categories:
  - javascript
date: 2019-01-22 15:12:08
---

# tagged template literals

자바스크립트에서 HTML을 조합해 사용하는 경우가 있다. json으로 받은 데이터들을 사용하는 경우이며 underscore와 같은 패키지를 사용하곤 한다. 

```javascript
const data = [
  {
    name: 'starbucks',
	order: true,
	menus: ['hand drip', 'americano']
  },
  {
	name: 'coffeebay',
    order: false,
  }
];

const message = `<h1>${data[0].name}</h1>
				 <p>menu: ${data[0].menus}</p>`

console.log(message);

// 결과
// "<h1>starbucks</h1>
//  <p>menu: hand drip,americano</p>"
```

data 배열 첫 번째 값의 name과 menus를 태그와 함께 출력해 보았다. 위의 예시는 간단하지만 조립해야 하는 태그와 데이터가 많아진다면 이런 방법으로 계속 할 수 있을까? 뿐만 아니라 데이터의 형식에 맞게 유동적으로 사용할 수 없다는 단점도 존재한다.

* 일일히 하나씩 해주어야 한다.
* 두 번째 객체 coffeebay에는 menus라는 항목이 없다.

형태에 따라 유동적으로 대응할 수도 없고 조립할 것들이 많아지면 수작업으로 하는 건 거의 불가능에 가깝다고 볼 수 있다. 또한 로직을 추가하는 것도 힘들 것이다. 조금은 더 유동적으로 사용하기 위해 함수를 작성해보자.

```javascript
const data = [
  {
    name: 'starbucks',
	order: true,
	menus: ['hand drip', 'americano']
  },
  {
	name: 'coffeebay',
    order: false,
  }
];

function messageFn(tags, name, menus){
	return (tags[0] + name + tags[1] + menus + tags[2])
}

const message = messageFn`<h1>${data[0].name}</h1>
				 <p>menu: ${data[0].menus}</p>`

console.log(message);

// 결과
// "<h1>starbucks</h1>
//  <p>menu: hand drip,americano</p>"
```

data[1]에는 menus가 없기 때문에 data[2]에 대해서 함수를 실행하게 되면 undefined가 출력된다. 객체의 키가 없을 경우를 대비하는 로직을 추가해서 반복문을 통해 모두 출력해보자.

```javascript
function messageFn(tags, name, menus){
    if (typeof menus === "undefined"){
        menus = "현재 메뉴를 준비중입니다...";
    }
    return (tags[0] + name + tags[1] + menus + tags[2])   
}

data.forEach(function(value){
    const message = messageFn`<h1>${value.name}</h1>
                              <p>menu: ${value.menus}</p>`

    console.log(message);
})
```

에러 없이 잘 작동한다. 마지막으로 콘솔 출력이 아닌 HTML에 뿌려보자.

```html
<div id="message"></div>
```

```javascript
data.forEach(function(value){
    const message = messageFn`<h1>${value.name}</h1>
                              <p>menu: ${value.menus}</p>`

    document.querySelector("#message").innerHTML(message);
})
```

최종 코드는 아래와 같다.

```javascript
const data = [
  {
    name: 'starbucks',
	order: true,
	menus: ['hand drip', 'americano']
  },
  {
	name: 'coffeebay',
    order: false,
  }
];

function messageFn(tags, name, menus){
    if (typeof menus === "undefined"){
        menus = "현재 메뉴를 준비중입니다...";
    }
    return (tags[0] + name + tags[1] + menus + tags[2])   
}

data.forEach(function(value){
    const message = messageFn`<h1>${value.name}</h1>
                              <p>menu: ${value.menus}</p>`

    document.querySelector("#message").innterHTML += message;
});
```


