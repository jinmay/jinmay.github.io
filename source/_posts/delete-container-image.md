---
title: delete-docker-image-container
tags:
  - docker
categories:
  - docker
date: 2018-06-19 16:11:52
---

# 도커 컨테이너 / 이미지 전체 삭제
도커 연습하다 보면 이것저것 이미지들을 많이 받기때문에 용량이 모자랄 때가 있는데 지금이 그 때이다.. 

생성해놓은 컨테이너도 많고 받아놓은 이미지들도 많아서 일일히 지우기 힘드니 한번에 지우고 싶을땐 다음과 같이 한다. 
**주의**
**모든 데이터가 날라간다!!**

~~~shell
# 컨테이너 전체 삭제
docker rm $(docker ps -a -q)

# 이미지 전체 삭제
docker rmi $(docker images -q)
~~~
컨테이너를 삭제하려면 해당 컨테이너가 stop된 상태여야한다.