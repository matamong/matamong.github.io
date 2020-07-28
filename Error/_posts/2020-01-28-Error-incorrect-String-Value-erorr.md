---
layout: post
title: DB Incorrect String Value 에러
description: >
  맞지않는 String 값 에러?
author: matamong
noindex: true
categories: [Error, DBMS, MySql]
tags: [error, DBMS, MySql]
comments: true
---

# **Incorrect String Value 에러**

## 원인
DB의 인코딩이 어딘가 맞지않아 생기는 전형적인 문제이다.
## 해결

**`my.ini`** 파일을 찾아서 아래와 같이 넣어주기

<br>


```ini
[mysqld]
datadir=C:/Program Files/MariaDB 10.4/data
port=3306
innodb_buffer_pool_size=2039M
character-set-client-handshake = FALSE 
character-set-server = utf8mb4 
collation-server = utf8mb4_unicode_ci
[client]
port=3306
plugin-dir=C:/Program Files/MariaDB 10.4/lib/plugin
default-character-set = utf8mb4
[mysql] default-character-set = utf8mb4
```

<br>

이모지까지 들어갈 수 있는 `utf8mb4` 로 인코딩해보았다.
<br>
기본적인 `utf-8` 인코딩은 아래와 같이 입력한다.


```ini
character-set-server = utf8

collation-server = utf8_general_ci

```

- SQL 프로그램을 재시작 한다.
- 테이블 인코딩 바꿔주기

```sql
ALTER TABLE 테이블 이름 convert to charset utf8(or utf8mb4);

```