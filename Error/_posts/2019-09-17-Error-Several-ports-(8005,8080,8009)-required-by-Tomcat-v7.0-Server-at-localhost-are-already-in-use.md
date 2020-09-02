---
layout: post
title: Several ports (8005, 8080, 8009) required by Tomcat v7.0 Server at localhost are already in use 에러
description: >
  **Several ports (8005, 8080, 8009) required by Tomcat v7.0 Server at localhost are already in use* 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, WAS, Tomcat]
tags: [error, WAS, Tomcat]
comments: true
---

# **Several ports (8005, 8080, 8009) required by Tomcat v7.0 Server at localhost are already in use**
# **Already Used Port**

## 증상
서버를 껐다 켰는데 `Several ports (8005, 8080, 8009) required by Tomcat v7.0 Server at localhost are already in use` 라는 경고문이 뜨면서 톰캣이 작동되지 않았다. <br>
## 원인
에러로그의 뜻은 `서버 포트를 이미 사용 중` 이라는 뜻으로, <br>
서버를 비정상적으로 종료하여 서버가 정상적으로 종료되지 않았거나 오류 등으로 인하여 여러 서버가 같은 포트를 사용하게 되면 나타나는 에러.  <br>

## 해결
### Window 의 경우
꼬여있던 포트를 찾아 강제로 종료해주고 다시 시작한다.
- `cmd`로 가서 가동 중인 포트를 찾는다.
```cmd
> netstat -a -n -o -p -tcp


TCP    0.0.0.0:****(포트번호)          0.0.0.0:0              LISTENING       *****(번호)
```
- 나열된 포트들 중 사용중인 톰캣의 포트번호(ex:8080) `pid`를 확인한다.
- 해당 포트를 종료한다.
```cmd
> taskkill /f /pid ****(pid번호)
```
- 종료되었다는 메시지를 확인한 후 다시 가동해본다.