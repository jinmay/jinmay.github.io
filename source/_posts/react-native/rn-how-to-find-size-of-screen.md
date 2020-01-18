---
title: 리액트 네이티브에서 스크린 사이즈 구하기
categories:
  - react-native
---

모바일 어플리케이션을 만들다 보면 각 기기마다 다른 스크린 크기 때문에 머리아픈 경우가 정말 많은 것 같다. 코드로서 화면 크기를 구할 수 있는 방법이 있을까 찾아보았고 정리해본다.

## 화면크기 구하기

**react-native의 Dimensions을 사용하면 된다.** 현재 열린 윈도우 사이즈를 통해 height와 width를 구할 수 있다.

```javascript
import { Dimensions } from 'react-native';

const chartHeight = Dimensions.get('window').height;
const chartWidth = Dimensions.get('window').width;
```

---

[참고]  
https://stackoverflow.com/questions/53796007/react-native-flex-layout-with-victorychart-chart-fit-the-parent-container
