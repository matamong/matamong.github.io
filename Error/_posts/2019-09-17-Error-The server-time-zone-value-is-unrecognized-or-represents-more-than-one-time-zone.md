---
layout: post
title: The server time zone value is unrecongnized or represents more than one time zone
description: >
  **The server time zone value** 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, DB, Spring]
tags: [error, DB, Spring]
comments: true
---

# **The server time zone value is unrecongnized or represents more than one time zone**

###Cause: org.springframework.jdbc.CannotGetJdbcConnectionException: Could not get JDBC Connection; nested exception is org.apache.commons.dbcp.SQLNestedException: Cannot create PoolableConnectionFactory (The server time zone value '' is unreconized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specific time zone valu if you want to utilize time zone support.)] with root cause

com.mysql.cj.core.exceptions.InvalidConnectionAttributeException: The server time value '' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

## 증상
Spring JDBC를 MySql과 연결하려고 했을 때 에러가 생김. <br>
에러 메시지를 보면 계속 time zone을 맞춰달라고 한다.
## 원인
MySql의 버전이 높을 경우 TimeZone과 SSL을 체크해줘야 하는데 안해줘서 생기는 오류
## 해결
`dataSourc()`로 가서 <br>
**`driver.Class.Name = "com.mysql.cj.jdbc.Driver`**
**`url = &verifyServerCertificate=false&useSSL=false&serverTimezone=UTC`** 를 추가해준다.


