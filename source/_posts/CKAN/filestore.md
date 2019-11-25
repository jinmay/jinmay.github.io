---
title: CKAN에 파일형태로 데이터셋 업로드 하기
tags:
  - ckan
  - filestore
categories:
  - ckan
date: 2018-10-26 18:06:25
---


# CKAN에 파일형태로 데이터셋 업로드

처음에 CKAN을 설치하고나서 테스트를 하기 위해 데이터셋 업로드를 하면 꽤나 당황스러움을 느낄 수 있다. 그 이유는 데이터 업로드가 링크를 통한 방법밖에 없을 뿐인데, 파일형태로 업로드를 하기 위해서는 다른 작업으르 필요로 한다. CKAN 문서에는 FileStore and file uploads라는 이름으로 나와있지만 서버를 준비할 때마다 겪는 과정이기 때문에 간단하게 정리해 본다.



## 파일 업로드 세팅

로컬 파일 스토리지에 CKAN의 FileStore을 설치하려면 아래와 같은 작업을 해야한다.

1. 업로드된 파일이 저장될 디렉토리를 생성해준다.

```ini
sudo mkdir -p /var/lib/ckan/default
```

**production.ini**의 **[app:main]** 항목에 **ckan.storage_path**를 보면 이미 /var/lib/ckan이 기본값으로 있는 것을 알 수 있다.

2. 설정파일에 경로를 변경해준다.[app:main]

```ini
ckan.storage_path = /var/lib/ckan/default
```

위에서 만들어준 경로를 저장할 폴더로 정해준다.

3. 권한 설정

```sh
sudo chown www-data /var/lib/ckan/default
sudo chmod u+rwx /var/lib/ckan/default
```

~~매우 중요한 부분이다. 처음 할땐 실수하지 않았는데, CKAN을 여러번 설치해봤다고 기억에 의존해서 넘어갔다가 permission deny를 보고 이게 뭔가 싶어서 세 시간을 날린적이 있었다.~~

정말 중요하다. 저장될 폴더의 권한 설정 하는 것을 까먹지 말자. CKAN을 우분투위에서 아파치를 통해 운영하고 있다면, 아파치의 유저(www-data)는 읽기 / 쓰기 권한을 꼭 가져야 한다.

4. 재시작

```sh
sudo service apache2 reload
```

