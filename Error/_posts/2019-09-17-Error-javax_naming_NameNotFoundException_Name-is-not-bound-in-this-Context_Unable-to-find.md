---
layout: post
title: javax_naming_NameNotFoundException_Name is not bound in this Context_ Unable to find 에러
description: >
  **javax_naming_NameNotFoundException_Name is not bound in this Context_ Unable to find** 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, WAS, Tomcat, DB]
tags: [error, WAS, Tomcat, DB]
comments: true
---

# **javax_naming_NameNotFoundException_Name is not bound in this Context_ Unable to find**

## 증상
`DAO`와 `JDCP`를 연동하려고 할 때 **`javax_naming_NameNotFoundException_Name is not bound in this Context_ Unable to find`** 이라는 경고문이 뜨며 에러가 났다.
## 원인
이름이 맞지 않아 찾을 수 없다는 뜻. <br>
`Tomcat`의 `Context.xml`에는`Oracle`로 등록을 했는데 <br>
`DAO`의 `context`객체에는 `Oracl`로 오타가 났었었다.
## 해결
이름을 제대로 적어주면 해결된다.
- `Contect.xml` 파일의 `Resource`에 저장했던 `name`을 그대로 `DAO`의 `Context`객체에 똑같이 넣어준다.