---
title: '[CKAN]harvest 실행하기'
tags:
  - ckan
  - harvest
  - harvesting
categories:
  - ckan
date: 2018-11-01 12:08:29
---

~~약 3일간 삽질끝에 어떻게 돌리는 지는 알게된것 같다.~~ 

CKAN 2.8 버전을 설치하고 ckanext-harvest 익스텐션을 인스톨하게 되면 구축한 CKAN 포털 페이지를 통해서 harvest dataset을 추가할 수 있게 된다. http://<ckan_url>/harvest의 주소로 접속하면 **Add Harvest Source** 라는 걸 볼 수 있다. 

Create Harvest Source의 페이지를 보게 되면, Source type이란 항목을 볼 수 있는데, 처음에는 CKAN밖에 없을 것이다. 구글링을 통해 CKAN의 harvest에 대해서 찾다보면 ckan instance 뿐만 아니라 CSW Server / DCAT JSON Harvester와 같은 source type을 보곤 하는데 활성화 하기 위해서 extension을 설치하고 플러그인을 활성화해야 한다. 

harvesting할 url과 다른 항목들을 적어주고 넘어가게 되면 admin의 manage에 들어가서 **Restart harvest** 를 해주어야 한다.



## harvesting 인터페이스

harvester interface를 구현하여 harvest 작업을 실행할 수 있다. harvest 과정은 세 단계로 구성되어 있다. 아래의 모든 작업은 CKAN의 virtualenv이 실행된 상태에서 실행되어야 한다!

* gather

두 번째 단계에서 fetch될 모든 리소스 identifier들을 컴파일 하는 과정이다.

```sh
paster --plugin=ckanext-harvest harvester gather_consumer --config=/etc/ckan/default/production.ini
```

* fetch

리모트 서버로부터 실제 객체 자료를 가져오거나 데이터 베이스에 저장하는 작업이다.

```sh
paster --plugin=ckanext-harvest harvester fetch_consumer --config=/etc/ckan/default/production.ini
```

* import

두 번째 단계에서 fetched된 리소스들에 대해서 필요한 모든 작업을 수행한다.

```sh
paster --plugin=ckanext-harvest harvester run --config=/etc/ckan/default/production.ini
```

gather - fetch - import 세 단계는 서로 다른 터미널을 켜놓고 하는 것을 추천한다.  



실습할 때는, 

* gather
* fetch

를 우선 실행해놓고 CKAN의 harvest를 실행하게 되면 알아서 인식해서 진행하게되는데 아직까진 이해를 못한 상태인 듯 싶다.



<hr>

<Reference>

[https://confluence.csiro.au/display/seegrid/CKAN+Harvesting+User+Guide](https://confluence.csiro.au/display/seegrid/CKAN+Harvesting+User+Guide)

[https://guidance.data.gov.uk/harvesting.html](https://guidance.data.gov.uk/harvesting.html)

[https://doc.arcgis.com/ko/hub/data/federating-with-ckan.htm](https://doc.arcgis.com/ko/hub/data/federating-with-ckan.htm)

[https://stackoverflow.com/questions/51701375/ckan-harvester-is-not-working/51726696#51726696](https://stackoverflow.com/questions/51701375/ckan-harvester-is-not-working/51726696#51726696)





