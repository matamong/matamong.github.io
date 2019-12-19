---
layout: post
title: 서블릿 포워드(Servlet forward)
image: https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.JPG
description: >
  Servlet의 forward 방식은 무엇이 있는지 공부해보았다.
author: matamong
noindex: true
categories: [Study, Servlet]
tags: [study, Servlet]
---

# **서블릿 포워드(Servlet forward)**

## **서블릿 포워드(Forward)**
* * *

서블릿의 포워드란, 각각의 서블릿끼리 혹은 서블릿과 JSP끼리 연동해서 작업해야 하는 경우 서로 작업을 전달하는 것을 말한다. 

포워드의 종류는 다음 네가지이다.
- **redirect**
- **Refresh**
- **location**
- **dispatch** (일반적인 포워드 방식)
<br><br>
이 네가지 포워드 방식을 알아보자.
<br><br>


## **redirect**
* * *
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C1.JPG) <br>

`HttpServletResponse` 객체의 `sendRedirect()` 메서드를 이용한다.<br>
- 웹 브라우저(클라이언트)에게서
>- **첫번째 서블릿**이 `요청`을 받으면
>- **첫번째 서블릿**이 다시 브라우저에게 `응답`을 보낸 후
>- 브라우저가 다시 **두번째 서블릿**에게 `요청`을 보낸다.
>- 요청을 받은 **두번째 서블릿**은 다시 브라우저에게 `응답`한다.

WAS에서 객체가 두번 생성되기 때문에 매핑 url이 달라지는 것을 볼 수 있다.<br>
아래 코드로 자세하게 확인해보자
<br><br>

**FirstSerlvet.java**
```java
@WebServlet("/first")  //Annotation으로 서블릿을 매핑해보겠다.
public class FirstServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		resp.sendRedirect("second"); //second로 매핑되어있는 서블릿으로 redirect하겠다.
	}
}

```


**SecondServlet.java**
```java
@WebServlet("/second")
public class SecondServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html><body>");
		out.println("Redirect 되었습니다. first->second");
		out.println("</body></html>");
	}

}

```
**결과** <br>
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/ForwardRedirect.JPG)
<br>
 FirstSerlvet에서 second로 매핑된 SecondServlet으로 Redirect한 코드는 브라우저를 한번 거쳐서 다시 `WAS`에서`HttpServletResponse`와 `HttpServletRequest` 객체를 생성하기 때문에 url이 변경된 것을 확인 할 수 있다.
<br><br><br>


## **Refresh**
* * *
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C2.JPG) 
<br>

`HttpServletResponse` 객체의 `addHadder()` 메서드를 이용한다.<br>
- 웹 브라우저(클라이언트)에게서
>- **첫번째 서블릿**이 `요청`을 받으면
>- **첫번째 서블릿**이 다시 브라우저에게 `응답`을 보낸 후
>- 브라우저가 다시 **두번째 서블릿**에게 `요청`을 보낸다.
>- 요청을 받은 **두번째 서블릿**은 다시 브라우저에게 `응답`한다.
>- `요청/응답` 방식은 `redirect`와 동일하다.

redirect와 요청/응답 방식은 비슷하다. 다만 Refresh 포워드 방식은 addHadder에 정보를 넣고 첫번째 서블릿을 잠깐 머물렀다가 다시 브라우저에 응답한다는 것이 차이이다.<br>
아래 코드로 자세하게 확인해보자
<br><br>

**FirstSerlvet.java**
```java
@WebServlet("/first")
public class FirstSerlvet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.print("1초간 머물 것 입니다");
		resp.addHeader("Refresh", "1;url=second"); //화면을 출력하고 1초 후에 second로 매핑된 서블릿으로 Refresh하겠다.
	}
}

```


**SecondServlet.java**
```java
@WebServlet("/second")
public class SecondServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html><body>");
		out.println("Refresh 되었습니다. first->second");
		out.println("</body></html>");
	}

}

```
**결과** <br> 
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/ForwardRefresh01.JPG) 
<br>
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/ForwardRefresh02.JPG) <br>
redirect와 똑같이 객체가 두번 생성되어 매핑된 url값이 달라졌지만 차이점은 첫번째 서블릿에서 잠시 머문다는 것이다.
<br><br><br>

## **location**
* * *

특이하게도 `JavaScript`의 `location` 객체를 이용한다.<br>
바로 확인해보자
<br><br>

**FirstSerlvet.java**
```java
@WebServlet("/first")
public class FirstServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.print("<script type='text/javascript'>"); 
		out.print("location.href='second';");    //JavaScript의 location 객체를 이용하여 second로 매핑된 서블릿으로 이동한다.
		out.print("</script>");
	}

}


```


**SecondServlet.java**
```java
@WebServlet("/second")
public class SecondServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html><body>");
		out.println("JavaScript의 location 객체로 Redirect 되었습니다. first->second");
		out.println("</body></html>");
	}

}

```
**결과** <br> 
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/ForwardLocation.JPG) <br>
이 또한 마찬가지로 매핑url은 first에서 second로 변한 것을 알 수 있다.이 말인 즉슨 location 포워드 방식도 `HttpServletRequest`객체와 `HttpServletResponse`객체가 새로 생성되었다는 것이다.
<br><br><br>

## **dispatch**
* * *
![dispatch.jpeg](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/%EC%8A%AC%EB%9D%BC%EC%9D%B4%EB%93%9C3.JPG)
<br>
포워딩이라고 하면 이 dispatch를 일반적으로 나타낸다. 
`RequestDispatcher` 클래스의 `forward()` 메서드를 이용한다.<br>
- 웹 브라우저(클라이언트)에게서
>- **첫번째 서블릿**이 `요청`을 받아
>- **첫번째 서블릿**이 `HttpServletRequest`와 `HttpServletResponse`객체에 정보를 담아서  직접 **두번째 서블릿**으로 `포워드`를 하면
>- **두번째 서블릿**이 객체를 받아 작동한 후 웹 브라우저에게 `응답`을 보낸다.

한 번 생성된 객체에 정보를 담아 두번째 서블릿에 포워드 했으므로 객체에는 변함이 없고 url도 변하지 않는다.
<br><br>

**FirstSerlvet.java**
```java
@WebServlet("/first")
public class FirstServlet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		RequestDispatcher dispatch = req.getRequestDispatcher("second");
		dispatch.forward(req, resp); //dispatch 클래스의 forward()메서드에 객체를 각각 담아 전송한다.
	}

}

```


**SecondServlet.java**
```java
@WebServlet("/second")
public class SecondSerlvet extends HttpServlet{
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		resp.setContentType("text/html; charset=utf-8");
		PrintWriter out = resp.getWriter();
		out.println("<html><body>");
		out.println("Dispatch를 이용하여 forward 된 서블릿입니다. first---");
		out.println("</body></html>");

	}

}

```
**결과** <br>
![img](https://raw.githubusercontent.com/matamong/Study/master/TIL/Web/Servlet/img/Servlet/ServletForwarding/ForwardDispatch.JPG) <br>
브라우저가 요청한 후 WAS에서 생성된 `HttpServletRequest`,`HttpServletRespons`객체는 첫번째 서블릿에서 정보를 담아 그대로 두번째 서블릿으로 포워드 되기 때문에 url도 변함없는 것을 확인 할 수 있다.
<br><br><br>

***
주로 Redirect와 forward(dispatch)로 쉽게 명칭하는 듯 하다...<br>





