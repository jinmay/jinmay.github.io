---
title: Django(장고) 로깅 기초
tags:
  - django
  - log
categories:
  - django
date: 2018-06-18 17:12:12
---

# 장고 로깅
현재 운영하고 있는 식단알리미의 사용자들이 어떠한 버튼을 누르는지 분석하기 위해 방법을 찾던 도중 사용자 입력에 대한 로그를 남기는 게 좋을 것 같아서 장고의 로깅하는 법에 대해서 간략하게 정리해보려고한다.

일단 장고의 로깅 시스템은 기본적으로 파이썬의 로깅 시스템을 그대로 따르면서 일부만 추가되었다고 한다. 파이썬의 로깅 모듈을 보면 **logger, handler, filter, formatter** 4가지의 컴포넌트를 정의하고 있다. 네 가지 로깅 컴포넌트들에 대해서 정리하자.

## 컴포넌트
### logger
logger는 로깅 시스템의 시작점이다. 로그 메세지를 처리하기 위해 메세지를 담아두는 저장소라고 생각할 수 있으며, 모든 로거는 이름을 가지고 있다. 또한 logger는 **logger level**을 갖고 있으며, 이는 로그 메세지의 중요도에 따라 자신이 어느 레벨 이상의 메세지를 처리할지에 대한 기준이 된다. 아래는 다섯 가지 log level 이다.

* **DEBUG** : 디버그 용도로 사용되는 정보. 로그 레벨의 최하위 수준이다
* **INFO** : 일반적이고 보편적인 정보(Django의 기본 로그 레벨값이다)
* **WARNING** : 문제점 중에서 덜 중요한 문제점이 발생 시 이에 대한 정보
* **ERROR** : 문제점 중에서 주요 문제점이 발생 시 이에 대한 정보
* **CRITICAL** : 치명적인 문제점 발생 시 이에 대한 정보. 로그 레벨의 최상위 수준이다

로거에 저장되는 메세지를 **로그 레코드(log record)**라고 부른다. 마찬가지로 **로그 레코드도 로그 레벨을 가진다.** 

메세지가 로거에 도착하면, 로그 레코드의 로그 레벨과 로거의 로그 레벨을 비교한다. 로그 레코드의 로그 레벨이 로거의 로거 레벨과 같거나 높으면 로거는 메세지 처리를 계속하게 된다. 만약 그렇지 않다면 해당 로그 메세지는 무시된다. 이와 같은 과정을 거쳐 로거는 로그 메세지를 핸들러에게 넘기게 된다.
```python
'loggers': {
        'board': {
            'handlers': ['console'],
            'level': 'DEBUG',
        }
    },
```

### handler
핸들러는 로거에 있는 로그 메세지에 어떠한 작업을 할지 결정하는 엔진이다. 로거로 부터 받은 메세지를 화면에 스트림으로 출력하거나 파일로 저장하거나 하는 등의 로그 동작을 의미한다. 핸들러도 로거와 마찬가지로 로그 레벨을 가지고 있다. 로그 레코드의 로그 레벨이 핸들러의 로그 레벨보다 더 낮으면 핸들러는 메세지를 무시하게 된다. 
```python
'handlers': {
        'console': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'format2',
        }
    },
```

### filter
로그 레코드가 로거에서 핸들러로 넘어갈 때, 필터를 사용해서 로그 레코드에 부가적인 제어를 할 수 있다. 기본 처리 방식은 로그 레벨을 지정하여 그 로그 레벨에 해당되면 관련 로그 메세지를 처리하는 것인데, 필터를 적용하게 되면 로그 처리 기준을 추가할 수 있다. 예를 들면, 로그 레벨이 ERROR인 로그 레코드 중에서 특정 소스로부터 오는 메세지만 핸들러로 넘길 수 있다. 

필터를 사용하면 로그 레코드를 보내기 전에 수정하는 것도 가능하다. 예를 들면, 어떤 조건에 맞으면 ERROR 로그 레코드를 WARNING 로그 레벨로 낮춰주는 필터를 만들 수도 있다. 필터는 로거와 핸들러 모두에게 적용할 수 있고, 여러 개의 필터를 체인 방식으로 동작시킬 수도 있다!

### formatter
로그 레코드는 최종적으로 텍스트로 표현된다. 이 때, 포멧터의 역할은 텍스트로 표현 시 임의의 포맷을 지정해주는 것이다. 포맷터는 보통 파이썬의 포맷 스트링을 사용하지만, 사용자 정의 포맷터도 만들어 사용할 수 있다. 
```python
'formatters': {
        'format1': {
            'format': '[%(asctime)s] %(levelname)s [%(name)s:%(lineno)s] %(message)s',
            'datefmt': '%d/%b/%Y %H:%M:%S',
        },
        'format2': {
            'format': '[%(levelname)s] %(message)s',
        },        
    },
```


## 로깅
로깅을 하기 위한 컴포넌트들이 준비되었다면 로그 메세지를 기록하기 위해 코드 내에서 로깅 메소드를 호출하면 된다.
```python
# logging 모듈 임포트
import logging

# 로거 인스턴스 생성
logger = logging.getLogger(__name__)

def index(request):
  logger.error('Something went wrong!') # ERROR 레벨 이상의 로그 레코드 로깅
  ...
```
로거 인스턴스는 이름을 가진다. 명시적으로 적어주어도 되지만 __name__ 특수 메소드를 통해서 명명해주는 것이 자주 사용되는 것 같다. 이와 같은 방법으로 로거 이름을 짓는다면 어떻게 출력이 될지 보고 싶을땐
```python
# in views.py

print(__name__) # 해당 모듈명 출력 : app.views
print(logger) # 로거 이름 출력 : <Logger app.views> 
```

다른 방식으로 로그 메세지를 계층화하고 싶으면 로거를 구분하는 이름을 도트(.)방식으로 명명해주면 된다.
```python
logger = logging.getLogger('project.interesting.stuff')
```

로거 객체(logger)는 각 로그 레벨별로 로깅 호출 메소드를 갖고 있다.
* **logger.debug()**
* **logger.info()**
* **logger.warning()**
* **logger.error()**
* **logger.critical()**

각 메소드들은 해당 레벨의 로그 메세지를 생성한다. 또한 두 가지 로깅 메소드를 추가로 제공하고 있다

* **logger.log()** : 원하는 로그 레벨을 정해서 로그 메세지를 생성한다
* **logger.exception()** : 익셉션 스택 정보를 포함하는 ERROR 레벨의 로그 메세지 생성


## 로깅 설정
네 가지 컴포넌트들(logger, filter, handler, formatter)을 설정해야 한다. 장고는 dictConfig 설정 방식을 기본으로 사용한다. 프로젝트의 **settings.py** 파일에 **LOGGING** 항목을 생성하여 설정을 하면 된다. LOGGING 속성을 정의하는 경우에 디폴트 로그 설정을 대체할 수도 있고, 디폴트 로그 설정과 병합할 수도 있다. 하는 방법은 LOGGING 설정에서 **disable_existing_loggers** 속성을 통해 지정하면 되는데 값이 True면 장고의 기본 로깅 설정을 대체(덮어쓰기)할 수 있다.

```python
LOGGING = {
  'version': 1, # 버전
  'disable_existing_loggers': True, # 장고의 기본 로깅 설정 활성화
  # 포맷터
  'formatters': {
      'format1': {
              'format': '[%(asctime)s] %(levelname)s [%(name)s:%(lineno)s] %(message)s',
              'datefmt': '%d/%b/%Y %H:%M:%S',
      },
      'format2': {
              'format': '[%(levelname)s] %(message)s',
      },
  },
  # 필터
  'filters': {
      'special': {
        '()': 'project.logging.SpecialFilter',
        'foo': 'bar',
      }
  },
  # 핸들러
  'handlers': {
      'console': { # 콘솔에 스트림 출력
          'level': 'DEBUG',
          'class': 'logging.StreamHandler',
          'formatter': 'format1'
      }
  },
  # 로거
  'loggers': {
      'board': {
          'handlers': ['console'],
          'level': 'DEBUG'
      }
  }
}
```

## 장고에 추가된 컴포넌트

### 로거
1. **django 로거**
모든 로그 레코드를 잡는 장고의 최상위 루트 로거이다. 이 루트 로거에 직접 로그 메세지를 보낼 수는 없다.

2. **django.request 로거**
http 요청 처리와 관련된 메세지를 기록한다. 5xx 응답은 ERROR / 4xx 응답은 WARNING 메세지로 발생한다. 이 로거에 담기는 메세지는 2개의 추가적인 메타 항목을 가진다. 
  * status_code : http 응답 코드
  * request : 로그 메세지를 생성하는 request 객체

3. **django.db.backends 로거**
데이터베이스와 관련된 메세지를 기록한다. 애플리케이션에서 사용하는 모든 sql 문장들이 이 로거에 debug 레벨로 기록된다. 성능상의 이유로, sql 로깅은 settings.DEBUG 속성이 True일 경우에만 활성화된다.
  * duration : sql 실행에 걸린 시간
  * sql : 실행된 sql 문장
  * params : sql 호출에 사용된 파라미터

4. **django.security.\* 로거**
사용자가 보안에 해를 끼칠수 있는 동작을 했을 경우 메세지를 기록한다. 

5. **django.db.backends.schema 로거**
데이터베이스의 스키마 변경 시 사용된 sql 쿼리를 기록한다.

### 핸들러
1. **AdminEmailHandler**
이 핸들러가 수신하는 모든 로그 메세지를 이메일로 사이트 관리자에게 전송

### 필터
1. **CallBackFilter**
콜백 함수를 지정해서 필터를 통과하는 모든 메세지에 대해 콜백 함수를 호출해준다. 콜백 함수의 리턴값이 False면 메세지 로깅은 더 이상 처리하지 않는다.

2. **RequireDebugFalse**
settings.DEBUG 속성이 False인 경우만 메세지 처리

3. **RequireDebugTrue**
settings.DEBUG 속성이 True인 경우에만 메세지 처리

### 장고 로깅 default
* django.request 로거
settings.DEBUG 속성이 False면 ERROR 또는 CRITICAL 레벨의 모든 메세지를 AdminEmailHandler 핸들러에게 보내주도록 되어있다

* django 루트 로거
django 루트 로거에 오는 모든 메세지는 DEBUG 속성이 True인 경우 콘솔로 보내지고, DEBUG 속성이 False인 경우 NullHandler로 보내서 무시되도록 기본값이 정해져있다.