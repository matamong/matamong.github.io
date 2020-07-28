---
layout: post
title: 중첩클래스(Nested Class)
image: https://image.flaticon.com/icons/svg/462/462295.svg
description: >
  클래스 안에 클래스! 중첩클래스를 알아보자.
author: matamong
noindex: true
categories: [Study, Java]
tags: [study, Java]
comments: true
---

# **중첩 클래스(nested Class)**

## **중첩 클래스란?**

### 다른 클래스 내부에 정의되는 클래스이다.

- **`스태틱 클래스`** : 다른 클래스 안에서 독립적으로 존재
- **`내부 클래스`**  : 자신이 정의된 클래스의 오브젝트 안에서만 존재
     - `멤버 내부 클래스` : 멤버 필드처럼 오브젝트 레벨에서 사용.
     - `로컬 클래스` : 메소드 레벨에서 사용.
     - `익명 내부 클래스` : 이름을 갖지 않는 클래스
<br><br>

## **익명 내부 클래스**
내부 클래스 중에서도 익명 내부 클래스는 이름을 갖지 않는 클래스이다. 
익명이라서 그냥 선언과 동시에 오브젝트가 생성이 된다.<br>
그러므로 상속할 클래스나 구현할 인터페이스를 **생성자 대신** 사용을 해서 이용하며 클래스를 계속 재사용하지않고 오직 구현에만 목적을 둘 경우에 유용하게 사용할 수 있다.(콜백)
<br><br>

```java
public interface TestInterface {
   public void writeCode();
}
```


```java
public class DoTest{
    
    TestInterface test = new testInterface(){
                            @Overrride
                            public void writeCode(){
                            return "Something"
                            }
                        }
}

```
이러면 TestInterface 타입의 test변수에는 새롭게 무언가가 정의된 testInterface가 담기겠지...? 다음에 다시 와서 천천히 봐야겠음


* * *
[ 참고도서 ] **`<<토비의 스프링 3.1>>`** - 지은이 김종민
