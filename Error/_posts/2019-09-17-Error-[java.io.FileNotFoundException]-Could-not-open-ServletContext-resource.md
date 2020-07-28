---
layout: post
title: java.io.FileNotFoundException - Could not open ServletContext resource
description: >
  **[java.io.FileNotFoundException] Could not open ServletContext resource**경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, Spring]
tags: [error, Spring]
comments: true
---

# **[java.io.FileNotFoundException] Could not open ServletContext [/WEB-INF/action-servlet.xml]**

## **증상**
Spring SimpleUrlController 이용해서 jsp 요청하다가 증상이 나타남. <br>
`HTTP Status 500 - Internal Server Error` 에러를 뿜으면서 에러가 났다. <br>
## **원인**
컨테이너에서 action-servlet.xml 을 찾지 못해서 에러가 났다.
## **해결**
`web.xml`에서 경로를 지정해주면 된다.
```xml
<servlet>
    <servlet-name>action</servlet-name>
    <servlet-class>org.springframework.web.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/action-servlet.xml</param-value>
    </init-param>
</servlet>
```