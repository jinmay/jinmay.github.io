---
title: 리눅스 명령어 - head / tail
tags:
  - linux
  - head
  - tail
categories:
  - linux
date: 2019-08-03 14:59:43
---

# 리눅스의 head / tail 명령어

파일의 앞 부분과 뒷 부분을 출력하는 명령어.

### head

파일 앞 부분의 10줄을 출력한다. 다양한 옵션을 사용할 수 있다.

````sh
head /etc/passwd # 기본이므로 상위 열 줄 출력
head /etc/passwd -n 5 # 위에서부터 다섯 줄 출력
head /etc/passwd -n -3 # 아래의 세 줄을 제외하고 모든 내용 출력
```

### tail

파일 뒷 부분의 10줄을 출력한다

> -n 옵션을 이용해서 몇 개의 라인을 출력할지 설정할 수 있다

```sh
tail /etc/passwd # 기본이므로 아래의 열 줄 출력
tail /etc/passwd -n 3 # 아래의 세 개의 줄만 출력
tail /etc/passwd -n +3 # 3번째 줄부터 모든 내용 출력
```

-f 와 -F 옵션을 사용하면 파일이 내용이 변경되는 것까지 추적할 수 있다

```sh
tail -f # 파일의 변경을 추적. 파일이 삭제된다면 더 이상 추적하지 않음
tail -F  # 파일의 변경을 추적. 파일이름이 변경되더라도 계속해서 추적한다
```
````
