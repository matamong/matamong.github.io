---
layout: post
title: Server Tomcat v8.5 Server at localhost failed to start 에러
description: >
  **Server Tomcat v8.5 Server at localhost failed to start** 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, WAS]
tags: [error, WAS]
comments: true
---

# **Server Tomcat v8.5 Server at localhost failed to start**

## 증상
톰캣 서버를 시작하려는데 `Server Tomcat v8.5 Server at localhost failed to start` 라는 경고문과 함께 서버 시작이 되지않았다.
## 원인
톰캣 서버에 문제가 생겨 발생하는 에러로 서버에 문제가 생기는 원인은 여러가지일 것이다.
## 해결
해결 방법에는 두가지가 있다. <br>
### 톰캣 서버 다시 추가
- `Window - Preference - Server - RuntimeEnviroment`에 들어가서 기존에 있던 `Tomcat` 을 제거하고 다시 추가
- 프로젝트 `Build Path - Server` 에서 톰캣추가
### 각기 다른 XML 파일로 publish
- `Perspective - Server` 에 들어가서 현재 있는 톰캣서버 더블클릭
- `Server Option`에서 `Publish module contexts to seperate XML files` 선택

