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
comments: true
---

# 서블릿 스코프(Servlet scope)
서블릿에서 변수를 지정하고 객체에 담아 포워드 하려면<br>객체가 어디까지 유지되는지를 반드시 알아야 마음대로 변수를 가지고 놀 수 있을 것 이다...<br>
서블릿 객체의 각 범위들을 공부해보자!<br>

# Servlet Scope

![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.PNG) <br>
서블릿의 스코프는 대략 이런 식으로 되어있다.<br>
JSP에서만 움직이는 Page Scope에서부터 애플리케이션 전체를 아우르는 Application Scope까지 하나씩 알아보자.
<br><br>


# Page Scope
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C2.PNG) <br>
JSP 내에서  <% %>로 선언되는 것은 해당 JSP 안에서만 사용 할 수 있는데 이런 개념이 `Page Scope`이다. <br>그래서 JSP의 지역변수처럼 활용이 가능하다. <br> <br>

![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C3.PNG) <br>
클라이언트(브라우저)의 요청이 있을경우 <br>
PageContext내장객체의 변수는 JSP 안에서만 머물게 된다. <br>
그러므로 포워드 될 경우 변수는 사라지게된다. <br><br>

# Request Scope
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C4.PNG) <br>
서블릿 컨테이너에서 만들어지는 `HttpServletRequest` 객체와 `HttpServletResponse`객체에 담기는 변수는 <br>
하나의 요청과 응답이 끝날 때 까지 머물게 되는데 이것을 
`Request Scope`라고 한다. <br><br>

![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C5.PNG) <br>
하나의 요청과 응답에 머무르기때문에 컨테이너 내에서 포워딩되어도 변수는 그대로 유지된다. <br><br>

# Session Scope
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C6.PNG) <br>
하나의 브라우저 내에서 공유되는 Scope이다. <br>
브라우저의 TAB에서 정보가 유지되는 것은 바로 이 객체의 변수가 유지되기 때문! <br><br>

![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C7.PNG) <br>
쇼핑몰의 장바구니를 생각하면 이해가 빨랐다. <br><br>

# Application Scope
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C8.PNG) <br>
하나의 애플리케이션 내에서 공유되는 Scope이다.<br>
하나의 애플리케이션 내의 변수를 여러 클라이언트와 공유 할 수 있다.<br><br>

![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletScope/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C9.PNG) <br>
잘 변하지 않고 모든 이들이 공통적으로 공유해야하는 것들은 이 `Application Scope`를 이용해야 한다.
<br><br>

* *  *
참고 사이트: <br>
[부스트코스:SCOPE란?](https://www.edwith.org/boostcourse-web/lecture/16708/)

 



