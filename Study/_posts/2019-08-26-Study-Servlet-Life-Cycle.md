---
layout: post
title: 서블릿의 생명주기(Servlet Life Cycle)
image: https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.JPG
description: >
  Servlet의 생명주기를 공부해보았다.
author: matamong
noindex: true
categories: [Study, Servlet]
tags: [study, Servlet]
comments: true
---

# 서블릿의 생명주기(Servlet Life Cycle)

**서블릿**의 생명주기를 알아보려고 한다.<br>
먼저, 서블릿이 동작을 할 때 까지의 과정을 알아보자.<br>
<br>

## **Servlet**이 동작하려면...<br>
* * *
![ppt1](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.JPG)

- 먼저 `클라이언트(Web Browser)`가 요청을 보낸다.
- 클라이언트의 요청을 `WAS`가 받아 `HttpServletRequest`객체와 `HttpServletResponse`객체가 존재하는지 확인 한 후,<br>
  존재하지 않으면 두 객체를 생성해서 웹 어플리케이션의 `서블릿 컨테이너`로 전달해준다.
- 이제 비로서 서블릿이 동작하며 서블릿 라이프사이클이 돌아가게된다.<br>
<br>

## **Servlet**의 생명주기 <br>
* * *
![ppt2](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C2.JPG)

**Servlet**은 
- **`init()`**
- **`service()`**
- **`destroy()`** 

이 세가지의 메서드를 돌며 살아간다.<br>
<br>

### **`init()`**<br>
* * *
![init()ppt](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C3.JPG)

- was에서 객체넘겨줌 -> 서블릿에서 딱 한 번 초기화 <br>

`init()` 메서드는 최초로 **딱 한 번 호출**된다.<br>
WAS는 요청이 오면 `HttpServletRequest`객체와 `HttpServletResponse`객체를 생성하여 서블릿 컨테이너에 "너 앞으로 이거 써~"하면서 넘겨준다. 이 때 요청이 여러번 온다고 생각해보자. 몇백만번의 요청이 올 때마다 WAS에서 몇백만번의 객체를 생성해버린다면 우리의 메모리는 죽어나갈 것이다! 그렇기 때문에 WAS는 메모리 자원을 아끼기 위해서 영리한 행동을 한다. 한 번 생성된 객체는 다음에 또 요청이 올 것을 대비하여 **메모리에 남겨둬버리는 것이다.** 그렇기 때문에 서블릿 컨테이너는 톰캣에서 이미 저장하려고 마음먹고 넘겨준 객체를 **초기화**만 시켜주면 된다. <br>
그렇기 때문에 WAS는 객체를 처음 생성 할 때 서블릿 컨테이너의  `init()`메서드를 딱 한번만 호출한다.<br> 
그러므로 라이프 사이클을 보면 서블릿을 처음 서버에 올렸을 때 `init()`이 호출된 뒤 서블릿에서 무언가를 변경한다거나해도 `service()`와 `destroy`만 반응 할 뿐, `init()`은 보이지 않는다.

<br>

### **`service()`**<br>
* * *
![service()ppt](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C4.JPG)

`service()`메서드는 실제로 제일 열일하는 메서드이다.<br>
`HttpServletRequest`,`HttpServletResponse` 이 두 객체를 가진 컨테이너가 `service()`메서드를 불러낸다.<br>
호출된 `service()`메서드 안에 있는 서브 메서드들이 요청에 맞게 호출된다.<br>
그리고 `HttpServletResponse`객체에 응답을 담아 보낸다.<br>
<br>

### **`destroy()`**<br>
* * *
![service()ppt](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletLifeCycle/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C5.JPG)

기존의 서블릿이 더 이상 필요없을경우 `destroy()`메서드가 호출된다.<br>
이는 `service()`메서드 내의 변경사항이 있을 경우에도 해당이 되기 때문에<br>
변경사항이 있을 경우 `destroy()`가 호출되고 다시 `service()`가 실행되는 모습을 볼 수 있다.<br>




 
