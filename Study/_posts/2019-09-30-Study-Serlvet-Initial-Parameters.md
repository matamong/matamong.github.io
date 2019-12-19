---
layout: post
title: 서블릿 스코프(Servlet scope)
image: https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.PNG
description: >
  Servlet의 scope에 대하여 공부해보았다.
author: matamong
noindex: true
categories: [Study, Servlet]
tags: [study, Servlet]
---

# **서블릿 초기화 파라미터(ServletConfig 와 ServletContext)**

자주 변하는 데이터를 서블릿에 넣어야 한다고 생각해보자. 서블릿 하나하나에 하드코딩을 하게 될 것이다. 이것은 유지보수를 생각하면 옳지 않은 방법이다. <br>그리하여 선대(?) 개발자들은 DD(배포 서술자)에 변화하기 쉬운 데이터를 설정해주고 서블릿에서는 그것을 파라미터로 받게 만들었다. 이제 데이터가 변경될 때는 DD만 수정하면 되는 것이다! 이 방법에 대하여 공부해보자.
<br><br><br>

## **ServletConfig 초기화 파라미터**

컨테이너는 서블릿을 하나 생성하려고 할 때 하나의 ServletConfig 객체를 만든다. 이 ServletConfig 객체를 이용하여 서블릿을 초기화 시켜줄 수 있다.<br>과정을 한 번 보자.
<br><br><br>
### **과정**
- **서블릿이 만들어질 때** 서블릿 컨테이너가 DD(web.xml)에 적혀진 파라미터를 읽는다.
     - ServletConfig는 DD에 파라미터를 설정할 때, `init-param`을 이용한다. 
     - ServletConfig는 파라미터를 `servlet` 항목에 포함시킨다.
- 컨테이너는 ServletConfig 객체를 만든 뒤, 읽었던 파라미터 정보를 ServletConfig에 넘겨준다.
-  컨테이너는 정보를 가지고 있는 ServletConfig 를 해당 Serlvet의 init()메서드의 파라미터로 넣어준다.
- 이제 해당 Servlet의 초기화가 완료됐다. 👍

<br><br><br>
### **어떻게 사용할까?**
<br>

*web.xml에 파라미터 등록*
```xml
<web-app ...>
    <servlet>
        <servlet-name>서블릿 이름 1</servlet-name>
        <servlet-class>패키지 경로.서블릿1 클래스이름</servlet-class>
        <init-param>
            <param-name>파라미터 이름</param-name>
            <param-value>파라미터 값</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>서블릿 이름 1</servlet-name>
        <url-pattern>/매핑주소</url-pattern>
    </servlet-mapping>
</web-app>
```

<br><br>

*서블릿 안에서는 아래와 같이 파라미터를 꺼낸다.*
```java
String a = getServletConfig().getInitParameterName("Param-name");
```
<br><br>

특정 서블릿에서만 값을 꺼낼 수 있기 JSP에서는 파라미터 값을 사용 할 수 없다. JSP에서 파라미터를 사용하고자 할 때에는 속성을 이용하여 JSP에게 값을 넘겨줘야만한다.(forwarding)

<br><br><br>


### **init()의 파라미터인 죄...**
이 방식엔 아쉬운 점이 있다. 바로 ServletConfig는 init()의 파라미터이기 때문에 **처음 딱 한 번만** 읽힌다는 것이다. (init() 관련 내용은 [ServletLifeCycle](https://matamong.github.io/study/servlet/2019-08-26-Study-Servlet-Life-Cycle/) 참고)<br>
그렇기 때문에 실행 중인 서블릿은 파라미터 쪽을 다시는 쳐다보지 않게되고 그 때 DD를 아무리 수정한다 한들 바뀌는게 없다!! <br>
만약 꼭 수정해야한다면 수정된 서블릿을 다시 초기화하기 위해서 서블릿을 재배포해야한다. 이 말은 즉, 데이터 하나 바꾸겠다고 서버를 내렸다가 올려야한다는 말이다! <br>
하드코딩은 피했지만 아직까지 아쉽기만 하다.

<br><br><br>

## **ServletContext 초기화 파라미터**
ServletContex는 ServletConfig에 포함되어있어 작동방식은 비슷하지만, 한 가지 큰 차이점이 있다.
<br> 바로, 하나의 서블릿이 아니라 **모든 웹 어플리케이션의 파라미터로 사용된다는 것**이다. 모든 웹 어플리케이션이라하면 하나의 서블릿 외에 다른 서블릿이나 JSP에서도 파라미터를 사용할 수 있다는 말이다!<br>
과정을 보자.

<br><br><br>

### **과정**

- **어플리케이션이 시작될 때** 서블릿 컨테이너가 DD(web.xml)에 적혀진 파라미터를 읽는다.
     - ServletContext는 DD에 파라미터를 설정할 때, `context-param`을 이용한다. 
     - ServletContext는 파라미터를 `servlet` 항목이 아닌 `web-app`에 포함시킨다.
- 컨테이너는 ServletContext 객체를 만든 뒤, 읽었던 파라미터 정보를 ServletContext에 넘겨준다.
-  이제 같은 웹 어플리케이션에 있는 모든 Servlet과 JSP가 ServletContext에 접근할 수 있다.👍

<br><br><br>
### **어떻게 사용할까?**
<br>

*web.xml에 파라미터 등록*

```xml
<web-app ...>
    <servlet>
        <servlet-name>서블릿 이름 1</servlet-name>
        <servlet-class>패키지 경로.서블릿1 클래스이름</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>서블릿 이름 1</servlet-name>
        <url-pattern>/매핑주소</url-pattern>
    </servlet-mapping>
    
    <!--이하 ServletContext 초기화 파라미터 -->
    <context-param>
        <param-value>파라미터 이름</param-value>
        <param-value>파라미터 값</param-value>
    </context-param>
</web-app>
```

<br><br>

*서블릿 혹은 JSP 안에서는 아래와 같이 파라미터를 꺼낸다.*

```java
ServletContext ctx = getServletContex();
String a = context.getInitParameter("파라미터 이름");

//혹은 한 줄로 할 수 있다.
String a = getServletContext().getInitParameter("파라미터 이름");
```
<br><br><br>

## **ServletCofig & ServletContext**
둘 다 무엇인지 알아봤으니 ServletConfig와 ServletContex의 차이점을 정리해보자. <br><br>

### 차이점
|  <center></center> |  <center>**ServletConfig**</center> |  <center>**ServletContex**</center> |
|:--------:|:--------:|:--------:|
|<center>**초기화 시기**</center> | <center> 서블릿을 생성할 때 </center> |<center>웹 어플리케이션이 시작될 때</center> |
|<center>**객체의 수**</center> | <center>서블릿 당 하나 </center> |<center>웹 어플리케이션 당 하나</center> |
|<center>**접근 범위**</center> | <center>특정 서블릿</center> |<center>모든 웹 어플리케이션</center> |
|<center>**DD 설정법**</center> | <center>`servlet`항목 안에 `init-param` </center> |<center>`web-app`항목 안에 `context-param`</center> |

<br><br>
### 공통점
- 컨테이너가 DD에 초기화 파라미터 값/쌍을 먼저 읽는다.
- 초기화 파라미터는 **String으로만 저장**할 수 있다.
- 리턴 값 또한 **String**이다.


<br><br> 만약, 초기화를 할 때 **String** 파라미터 초기화 외에 다른 초기화행동도 하고싶다면...?(ex: 초기화 시 데이터베이스 연결을 위한 **객체**생성 등..)<br>
속성(Attribute)을 이용하는 **[SerlvetContextListener]()** 를 공부하자.



<br><br><br>
***
[참고서적] `<<Head First Servlet & JSP>>`<br>
 -지은이 케이시 시에라, 버트 베이츠, 브라얀 바샴 <br>
[참고 사이트]`https://tomcat.apache.org`의 ServletContext API <br>
 https://tomcat.apache.org/tomcat-5.5-doc/servletapi/javax/servlet/ServletContext.html <br>
 [참고사이트]`https://javabeginnerstutorial.com`의 Servlet Context tutorial for Java beginners <br>
 https://javabeginnerstutorial.com/servlet-2/servlet-context-tutorial-for-java-beginners/ <br>
