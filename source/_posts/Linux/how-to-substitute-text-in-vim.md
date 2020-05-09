---
title: '[vim]문자열 치환하기'
categories:
  - linux
---

한 번에 문자열을 치환할 수 있는 방법을 정리한다.  

## 문자열 치환

```vim
:%s/<변경대상 문자열>/<변경할 문자열>
:%s/<변경대상 문자열>/<변경할 문자열>/<옵션>
```

**Tip: %s의 s는 *치환하다*라는 뜻을 가진 substitute를 의미한다고 생각하면 기억하기 쉬울 것 같다.**

예를 들어 문서에 *pick*이라는 문자열을 *edit*변경 하고자 한다면 아래와 같이 할 수 있다.

```vim
:%s/pick/edit
```

## 옵션

- g: global - 파일 전체를 대상으로 한다.
- i: ignore case - 대소문자를 구분하지 않는다.
- c: confirm - 검색된 문자열에 대해서 치환할지 말지 물어본다.

### 예시

1. 문서 전체에서 *pick*를 *edit*으로 변경

```vim
%s/pick/edit/g
```

2. 문서 전체에서 *pick*를 *edit*으로 변경하는데, 각 문자열이 검색될 때마다 변경할 건지 묻기

```vim
%s/pick/edit/gc
```

---
[참고]  
https://overegoz.tistory.com/659  
http://gypark.pe.kr/wiki/Vi%EB%A1%9C%EB%AC%B8%EC%9E%90%EC%97%B4%EC%B9%98%ED%99%98%ED%95%98%EA%B8%B0  