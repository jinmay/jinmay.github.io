---
title: Ubuntu locale 한글로 설정하기
categories:
  - linux
date: 2019-11-04 13:33:33
tags:
---


# Ubuntu locale 한글로 설정하기

> 로케일 설정은 언어 뿐만 아니라 날짜 / 시간과 같은 표현에도 영향을 미친다

aws의 우분투를 사용하던 중 git log의 한글이 깨져서 보이는 현상이 발생했다. locale 명령어를 통해 보니 en-US.UTF-8 이었기 때문에 한국어 UTF-8로 바꾼 과정을 정리해본다.

## Locale 변경하기

#### 현재 시스템 확인

```sh
locale
```

우선 현재 시스템의 locale은 아래의 명령어를 통해 확인할 수 있다.

#### 한글 패키지 설치

```sh
sudo apt-get install language-pack-ko
```

#### 시스템 파일 수정

```sh
# vi /etc/default/locale

LANG=ko_KR.UTF-8
LC_MESSAGES=POSIX

# 출력
# LANG=ko_KR.UTF-8
# LANGUAGE=
# LC_CTYPE=ko_KR.UTF-8
# LC_NUMERIC="ko_KR.UTF-8"
# LC_TIME="ko_KR.UTF-8"
# LC_COLLATE="ko_KR.UTF-8"
# LC_MONETARY="ko_KR.UTF-8"
# LC_MESSAGES=POSIX
# LC_PAPER="ko_KR.UTF-8"
# LC_NAME="ko_KR.UTF-8"
# LC_ADDRESS="ko_KR.UTF-8"
# LC_TELEPHONE="ko_KR.UTF-8"
# LC_MEASUREMENT="ko_KR.UTF-8"
# LC_IDENTIFICATION="ko_KR.UTF-8"
# LC_ALL=
```

시스템의 언어 설정 파일은 기본적으로 /etc/default/locale에 저장이 되어있다. 따라서 이 부분을 위와 같이 편집해주고 서버 재부팅 또는 재접속을 해보자!!

#### 확인

```sh
locale
```

---
[참고]
<https://www.manualfactory.net/10349>
<https://beomi.github.io/2017/07/10/Ubuntu-Locale-to-ko_KR/>