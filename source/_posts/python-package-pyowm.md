---
title: PyOWM를 이용하여 현재기온 받아오기
tags:
  - package
  - pyowm
categories:
  - python
date: 2018-01-09 11:21:25
---


현재 간단하게 운영하고 있는 서비스에 현재기온을 알 수 있는 기능을 추가하고 싶어 api를 찾던 중 **openweathermap**를 발견했다. api doc을 읽으면서 그냥 사용해도 무방할 것 같았지만 왠지 래핑된 파이썬 패키지가 있을거 같아서 찾아보았더니 역시나.. 있었다. official doc은 물론 깃허브에 설명이 너무 잘 되어있어서 사용하는 데에는 크게 무리 없었지만 처음으로 오픈소스를 찾아본 기념으로 정리하려고 한다.

# PyOWM
일단 OWM이 뭔지 알아야 할 것 같다. OWM이란? **OpenWeatherMap**은 현재날씨 뿐만아니라 예보 풍향, 기온 등을 알려주는 온라인 서비스이다. 총 다섯가지 티어로 나누어져있고 프리티어도 존재하기 때문에 무료로 사용할 수 있다. 공식 페이지를 통해 각 티어간의 차이를 알 수 있을 것이다. 

나의 경우에는 현재기온만을 필요로 했기때문에 프리티어면 충분했다. owm의 장점으로는 api doc이 설명이 자세하다는 것과 사용하는 것도 편리하다는 것이다. 하지만 더 나아가서 파이썬 사용자들을 위해 누군가가 더 편리하게 만들어서 PyOWM이라는 파이썬 패키지를 제공하고 있는 것이다. 파이썬2의 2.7과 3.2버전 이상을 지원하고 있으며 웹 프레임워크 Django1.10이상의 모델에도 사용할 수 있다.

### 설치
1. pip를 이용한다
```bash
pip install pyowm
```
  매우 편리하다. 쉽다.


## 사용법
**일단 owm이 제공하는 api를 이용하기 위해서는 공식 페이지로 들어가 api key를 받아야한다.** 
~~물론 영어로 적혀있지만 간단하기 때문에 쉽게 할 수 있을 것이다.~~

*주의할 점*
*서비스에 가입하고 api key를 받았더라도 바로 사용할 수 없다. 공식 안내에 따르면 free tier 계정과 startup tier 계정은 받은 api key를 활성화하는 데에 10분이 소요된다고 하니 발급받고 10분 뒤에 사용하도록 한다.*

설치한 패키지를 임포트 하는것으로 시작하면 된다.
```bash
# 아래 어느것으로 해도 무방하다
# import pyowm
from pyowm import OWM 
```

임포트 한 뒤 OWM 객체를 생성해야 한다.
```bash
owm = OWM(API_KEY)
```
어떠한 종류의 api를 구독하는지 명시하지 않았다면 free tier로 간주한다. 유료api를 구독하고 있다면 아래와 같이 하자.
```bash
owm = OWM(API_KEY, subscription_type='pro') # pro tier를 구독하는 예시
```

사용하려는 owm api 버전도 지정할 수 있다. 만약 명시하지 않으면 가장 최신버전의 api를 사용한다.
```bash
owm = OWM(API_KEY, version='2.5') # owm api version == 2.5
```

지금까지 했다면 최소한의 준비는 됐다. 도시이름을 통해 해당도시의 날씨상황을 가져와보자. 현재 날씨를 받아오기 위해 검색하는 것은 매우 간단하다. 날씨를 알고싶은 지역의 이름을 OWM 객체에 전달해주면 된다. "지명"으로 해도 좋고 "도시ID"와 "위도/경도"로도 검색 할 수 있다.
```bash
observation = owm.weather_at_place('Seoul,KR')
```
입력한 지역과 매칭되는 날씨 정보를 포함한 **observation 객체**가 리턴된다. observation 객체는 두 가지 또다른 객체를 포함하고 있다. **날씨와 관련된 데이터를 포함한 Weather 객체와 지역에 대한 데이터를 가진 Location 객체 이다.** 

아래와 같이 하면 Weather 객체를 가져올 수 있다.
```bash
seoul_weather = observation.get_weather()
print(seoul_weather)   # <Weather - reference time=2013-12-18 09:20, status=Clouds> 를 출력한다.
```
 
더 디테일한 날씨정보를 가져오기 위해 아래 예시코드를 참고하자 : 
```bash
seoul_weather.get_wind()                  # {'speed': 4.6, 'deg': 330}
seoul_weather.get_humidity()              # 87
seoul_weather.get_rain()                  # Get rain volume
seoul_weather.get_clouds()                # Get cloud coverage 
seoul_weather.get_snow()                  # Get snow volume
seoul_weather.get_wind()                  # Get wind degree and speed
seoul_weather.get_pressure()              # 기압

seoul_weather.get_temperature()           # 온도(캘빈온도)
seoul_weather.get_temperature('celsius')  # {'temp_max': 10.5, 'temp': 9.7, 'temp_min': 9.0}

seoul_weather.get_status()                # Get weather short status ex) 'Clouds'
seoul_weather.get_detailed_status()       # ex) 'Broken clouds'
seoul_weather.get_weather_code()          # owm에서 사용하는 상태코드 - ex) 803
seoul_weather.get_sunrise_time()          # 일출시각 ex) 1377862896L
seoul_weather.get_sunset_time('iso')      # 일몰시각(iso시간) ex) '2013-08-30 20:07:57+00'
```

위에서 이미 얘기했듯이 observation 객체는 Location 객체도 가지고 있다. 아래는 Location 객체를 가져오는 코드이다.
```bash
location_seoul = observation.get_location()
print(location_seoul)
```

Location 객체도 다양한 속성이 있다.
```bash
location_seoul.get_name()    # 'Seoul'
location_seoul.get_lon()     # 경도
location_seoul.get_lat()     # 위도
location_seoul.get_ID()      # 도시ID
```


~~실시간 날씨현황뿐만 아니라 예보기능도 있지만 아직 사용해보지 않은 관계로 추후에 정리해보자!!~~


------
[참고]
<https://github.com/csparpa/pyowm>
<https://github.com/csparpa/pyowm/blob/master/pyowm/docs/usage-examples.md>
<https://openweathermap.org/current>