---
title: rn-how-to-use-websocke-in-react-native
---

'리액트 네이티브에서 웹 소켓 사용하기'

리액트 네이티브에서 웹 소켓을 사용하기 위해서 node.js의 socket.io 또는 기본으로 제공하는 WebSocket을 이용해야한다.

처음에는 socket.io를 이용하여 구현하려고 시도했는데 잘되지 않아서 리액트 네이티브의 공식문서에서 제시하는 것처럼 WebSocket을 이용하여 구현하였다.

## WebSocket 사용법

일단 웹 소켓 객체를 생성해야 하며, 인자로 주소를 적어준다.

```javascript
const ws = new WebSocket('ws://<주소>');
```

각 상황에 사용할 함수들을 사용하면 된다.

```javascript
// 소켓 연결 시
ws.onopen = () => {
	ws.send('something'); // 메세지 전송
};

// 메세지 수신
ws.onmessage = e => {
	console.log(e.data);
};

// 에러 발생시
ws.onerror = e => {
	console.log(e.message);
};

// 소켓 연결 해제
ws.onclose = e => {
	console.log(e.code, e.reason);
};
```

---

[참고]  
https://facebook.github.io/react-native/docs/network#websocket-support
