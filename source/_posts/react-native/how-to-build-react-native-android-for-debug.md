---
title: 리액트 네이티브에서 테스트를 위한 안드로이드 APK 빌드하기
categories:
  - react-native
---

실 기기에 테스트해볼 용도로 apk를 만드는 과정을 정리한다. 사실, 구글 플레이 스토어를 보면 스토어에 올려서 open하지 않고 테스트할 수 있다. 인증을 거치지않는다.

간단히 정리를 해보면 다음과 같다.

1. bundle 파일 생성
2. 안드로이드 스튜디오로 프로젝트 열기
3. apk 생성

## 1. bundle 생성

프로젝트 폴더에서 다음과 같이 입력한다. 각 옵션들이 제대로 적혀 있는지 본다. 간혹 코드의 시작점을 바꾸는 경우가 있는데, 만약 시작점은 변경했다면 --entry-file을 변경해주어야 한다.

```shell
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
```

## 2. 안드로이드 스튜디오로 프로젝트 열기

개발중인 리액트 네이티브 앱의 프로젝트 폴더를 연다.

## 3. apk 생성

프로젝트를 열었다면 안드로이드 스튜디오의 상단 Build 옵션에 들어가서 Build Bundle / APK를 확인한다. **APK를 생성하기 위해 Build APK 클릭!**

![image](https://user-images.githubusercontent.com/13075035/72409648-b726de80-37a9-11ea-84fc-05de0e2c19ad.png)

## 4. 생성된 APK 확인

APK 빌드가 완료되면 --assets-dest 옵션에서 명시한 경로에 app-debug.apk 파일이 생성된다.

- 경로 : /android/app/src/main/res/
- 파일명 : app-debug.apk

---

[참고]  
https://webisfree.com/2018-09-17/react-native-standalone-%EB%B0%A9%EB%B2%95%EC%9C%BC%EB%A1%9C-apk-%EB%A7%8C%EB%93%A4%EA%B8%B0
