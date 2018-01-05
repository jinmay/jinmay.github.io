---
categories:
  - ETC
tags:
  - hexo
title: hexo와 Github Page를 이용한 블로그
---

지난 17년에는 블로그가 아닌 Github를 통해 간단하게 마크다운 파일로만 그날그날 공부해왔던 것들을 정리했었다.
당시에도 지킬(Jekyll)을 이용하여 블로그를 할지 간단한게 할지 고민했었는데 올해부터는 hexo와 github page를 이용하여 공부하는 것들을 정리해보려고 한다.

github page와 자주사용되는 정적사이트 생성기로는 지킬(Jekyll)이 있다. Jekyll은 github page에서 기본적으로 지원을 하고 있으며 ruby로 되어 있다고 한다. 그리고 다른 정적사이트 생성기로는 **hexo**가 있다. hexo는 요즘 frontend와 backend을 막론하고 자주 사용하는 javascript로 되어있으며 jekyll보다 이쁜 테마가 많다고 알려져있다. 예전에 jekyll을 이용하여 잠깐 블로깅을 해본적이 있는데, 이번에는 hexo를 이용하여 해보기로 하며 일회성 이벤트가 아니라 꾸준히 할 수 있기를 기대한다.

hexo를 설치하기 이전에 필요한 준비물이 있다.
1. node
2. git

따로 인스톨패키지를 지원하는 것이 아니기 때문에 npm(Node.js package manager)을 이용하여 설치해야만 한다.

### 설치
1. hexo-cli 설치하기
~~~bash
npm i hexo-cli -g
~~~
  위 와 같이 입력하면 전역적으로(global) hexo가 설치된다.
  
2. 블로그 생성 
~~~bash
hexo init <blog_name> # 블로그 이름을 적어준다
cd <blog_name>
npm install # 약자는 hexo i
~~~
  init을 하게되면 지정한 이름으로 블로그 폴더가 생성되고 내부에는 블로그에 필요한 파일들이 마구마구 생성된다.
  cd를 이용해 해당 디렉토리에 들어가고 패키지를 인스톨해주면 준비는 끝이다.

3. 로컬서버 실행
~~~bash
hexo server # 약자는 hexo s
~~~
  배포하기 전에 로컬서버를 통해서 먼저 확인한다. 기본적인 주소는 http://localhost:4000 이다. 
  source/_posts 폴더 내부의 md파일이 보여지게 되며 최초생성시에는 hello world와 hexo를 소개하는 페이지가 보일 것이다.


### 설정하기
hexo의 전반적인 설정은 **_config.yml**에서 할 수 있다. 꽤나 직관적인 영어로 작성되어 있기 때문에 설정하는 데에 큰 어려움은 없다. 짧막한 예시는 아래의 코드와 같다
~~~yml
# Writing
new_post_name: :title.md # File name of new posts
default_layout: draft # layout 기본설정 -> hexo new [layout] 생략 시 draft 모드가 기본값이다
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:
~~~

### 글 작성
글을 작성하는 방법에 자주 사용되는 것은 **초고(draft)**를 작성하고 **발행(publish)**하는 것이다.

초고를 작성하기 위해 source/\_drafts 폴더를 그리고 발행을 위해 source/\_posts 폴더를 만드는 방법도 있지만, 아래와 같은 명령어를 통해 폴더와 파일을 자동으로 생성하는 것이 훨씬 간편하다.
~~~bash
hexo new [layout] <document_name>
~~~

설정파일의 **default_layout**을 통해 기본 레이아웃을 설정할 수 있다.

~~~yml
# _config.yml
default_layout: draft
~~~

나의 경우 발행할 글을 바로 작성하는 것보다 초고를 쓰고 읽어본 뒤 발행하는 편이기 때문에 default_layout을 draft로 주었다. 만약 "settings-hexo"라는 이름으로 draft를 생성하고 싶으면 다음과 같이 하면 된다.
~~~bash
hexo new draft "settings-hexo"
# 또는
hexo new "settings-hexo"
~~~

source/_drafts 폴더에 **settings-hexo.md** 라는 마크다운 파일이 생성되었을 것이다. 게시글을 작성했으면 게시글을 포스팅하기 위해 발행할 차례이다. 발행(publish)하는 명령은 다음과 같이 하면 된다.
~~~bash
hexo publish <file_name>
~~~
source/_posts 폴더 내에 **settings-hexo.md**가 생겼을 것이고 로컬에서 확인하기 위해 **hexo s**을 실행해 본다.

다음으로 실제 github page로 배포할 차례이다. 배포 이전에 설정파일에 해주어야할 작업이 있다.
1. 설정파일 수정
2. 패키지 설치

### 설정파일 수정
~~~yml
# _config.yml

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy: 
  type: git
  repo: <address>
~~~
설정파일의 맨 아래를 보면 위의 코드와 같은 것이 있다. github page를 이용하기 때문에 **type에는 git을 적어주고 repo에는 github page 주소를 적어주어야 한다.** 

### 패키지 설치
~~~bash
npm install hexo-deployer-git --save
~~~

설정파일도 수정하고 패키지도 설치했다면 준비는 완료된 것이다. **배포를 해보자!**

### 배포하기
~~~bash
hexo deploy -g
~~~
매우 간단하다. 이전에 입력해놓은 주소로 손쉽게 배포할 수 있다. 깃허브에 올라가는 파일은 source파일이 아닌 public 폴더가 올라가게 되며, 문서의 변경내용을 관리하고 싶다면 추가적으로 해야할 일이 있다.

### 마크다운파일의 변경내용 관리하기
새로운 브랜치를 생성해야 한다. 실제로 블로그에 보이는 파일은 master 브랜치를 통해 배포하고 파일의 변경내용은 새로만든 브랜치에서 하는 것이다. **gh-pages**라는 브랜치를 새로 생성하자.
~~~bash
git checkout -b gh-pages
~~~
위의 명령은 gh-pages라는 브랜치를 생성하고 바로 브랜치를 변경해준다. gh-pages 브랜치에 푸시를 하고 hexo deploy 명령어를 통해 배포를 하면 문서파일 관리와 동시에 블로그로 배포할 수 있게 된다!!