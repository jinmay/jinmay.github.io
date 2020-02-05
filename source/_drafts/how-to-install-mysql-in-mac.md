---
title: how-to-install-mysql-in-mac
---

'[MySQL]맥북에서 mysql 설치하기'

맥북에서 mysql을 설치하는 방법을 정리한다.

## Homebrew를 통한 설치

### 설치

brew 업데이트를 하고 설치를 진행한다.

```sh
brew update
brew install mysql
```

### 서버 시작

```sh
mysql.server start
```

### 서버 정지

```sh
mysql.server stop
```

### root 비밀번호 설정

```sh
mysql_secure_installation
```

root 비밀번호를 설정하기 위해 위의 명령어를 입력하게 되면 몇몇의 질문을 받게 된다. 질문에 대한 내용은 다음과 같다.

```sh
# root 비밀번호 입력
Enter password for user root:

# 비밀번호 보안을 위한 컴포넌트 사용 여부
VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

# 기존에 root 패스워드를 만들었다면 변경할 것인지
Change the password for root ? ((Press y|Y for Yes, any other key for No) :

# 익명 사용자를 사용할 것인지 여부.
# no를 입력하면 익명사용자를 그대로 두며 mysql 명령어만으로 접속 가능
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) :

# localhost 이외의 주소에서 root 사용자로 접속 가능한지 여부
# yes를 입력하면 외부에서 root 사용자로 접속 불가
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) :

# 테스트 DB 삭제 여부
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.

Remove test database and access to it? (Press y|Y for Yes, any other key for No) :

# 권한관련 테이블 리로딩 여부
# 권한 변경을 수행했다면 yes를 입력
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) :
```

### charset 확인

utf8을 사용하는 게 정신적으로 이로움으로 꼭 확인해본다. **mysql 접속 후 명령어 실행!**

```mysql
status;

Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	utf8mb4
Conn.  characterset:	utf8mb4
```

---

[참고]  
https://github.com/helloheesu/SecretlyGreatly/wiki/%EB%A7%A5%EC%97%90%EC%84%9C-mysql-%EC%84%A4%EC%B9%98-%ED%9B%84-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
