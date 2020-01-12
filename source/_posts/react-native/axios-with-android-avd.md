---
title: 안드로이드 AVD 에뮬레이터를 통해 axios 사용할때 발생하는 문제점
categories:
  - react-native
---

리액트 네이티브를 통해 앱 개발을 할때 http 패키지로서 보통 axios를 사용하는 것 같다. AVD를 띄우고 로컬호스트로 돌아가는 장고 서버에 HTTP 요청을 하려고 했는데 항상 network error를 뱉었다. **유효하지 않은 요청이 아니라 아예 요청 자체가 들어가지 않는 현상**이었다. 정확한 문제의 원인은 찾지 못했지만 AVD를 통해 안드로이드 시뮬레이터를 사용중이라면 localhost의 주소를 아래의 주소로 변경해보라는 답을 발견했고 문제를 해결할 수 있었다.

**10.0.2.2**

---

[참고]  
https://github.com/axios/axios/issues/973
