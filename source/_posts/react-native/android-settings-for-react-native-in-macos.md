---
title: React Native를 위한 안드로이드 설정하기
categories:
  - react-native
date: 2019-12-28 15:17:11
---

**맥북에서 안드로이드 스튜디오를 이용해 안드로이드 시뮬레이터를 열기까지의 과정을 정리한다.**

## 1. node와 watchman 설치

리액트 네이티브를 위해 노드가 필요하다. 또한 watchman은 페이스북에서 만든 파일시스템의 변경사항을 추적할 수 있는 툴이다. 더 나은 퍼포먼스를 위해서 설치하는 것은 매우 추천한다고 한다.

```bash
brew install node
brew install watchman
```

## 2. JDK 설치

맥을 사용한다면 brew를 통해 JDK를 설치할 수 있으며 오라클JDK 또는 OpenJDK를 설치하면 된다. 본 정리에서는 OpenJDK8을 설치한다.

```shell
brew tap AdoptOpenJDK/openjdk
brew cask install adoptopenjdk8
```

> 사실 먼저 9버전을 설치하고 시뮬레이터를 사용하려고 했는데 알 수 없는 에러로 인해서 실패했다. JDK 버전을 8로 내렸더니 문제 없이 사용할 수 있었다.

## 3. 안드로이드 개발 환경 설정

### 3-1. 안드로이드 스튜디오 설치

안드로이드 스튜디오를 설치하고 SDK 및 AVD를 설치해주어야 한다. 안드로이드 스튜디오 설치 시에 custom 옵션을 통해 아래의 네 가지 옵션을 체크해주어야 한다.

- Android SDK
- Android SDK Platform
- Performance (Intel ® HAXM) (See here for AMD)
- Android Virtual Device

### 3-2. 안드로이드 SDK 설치

기본적으로 안드로이드 스튜디오를 설치할때 가장 최신 버전의 안드로이드 SDK를 설치하게 된다. 하지만 리액트 네이티브 앱을 빌드하기 위해서 안드로이드 9(Pie) 버전의 SDK가 필요하다. 다른 버전의 안드로이드 SDK가 필요하다면 SDK Manager를 통해 설치할 수 있다.

아래의 SDK를 설치한다. 만약 Intel x86과 같은 옵션이 보이지 않는다면 **Show Package Details** 을 눌러서 디테일 페이지를 살펴보면 된다.

- Android SDK Platform 28
- Intel x86 Atom_64 System Image

### 3-3. ANDROID_HOME 경로 지정하기

안드로이드에 관련된 경로를 PATH에 추가해주어야 한다. 본인이 사용중인 쉘의 설정파일에 등록하자.

```bashrc
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

### 3-4. 시뮬레이터 설정

시뮬레이터로 사용하고자 하는 디바이스를 추가한다.

---

## 4. 마무리

새로운 리액트 네이티브 프로젝트를 생성하고 시뮬레이터를 사용할 수 있다.

```bash
# 리액트 네이티브 프로젝트 생성
npx react-native-cli init sampleProj

cd sampleProj
npx react-native run-android # 안드로이드 오픈

```

[참고]  
https://facebook.github.io/react-native/docs/getting-started.html#android-development-environment
