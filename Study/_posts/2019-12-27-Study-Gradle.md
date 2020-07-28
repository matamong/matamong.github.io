---
layout: post
title: Gradle 이란?
image: https://gradle.org/images/gradle-knowledge-graph-logo.png?20170228
description: >
  Gradle은 무엇일까? 어떻게 설치할까?
author: matamong
noindex: true
categories: [Study, Gradle]
tags: [study, Gradle]
comments: true
---

# **Gradle**

## **Gradle이란?**
그루비(Groovy) 기반의 빌드 자동화 오픈 소스

- 빌드 스크립트를 xml이 아닌 그루비(Grooby)로 작성한다.
   - 그루비(Groovy)란?
      - 특정 도메인에 특화된 언어인 DSL(Domain Specific Language)
      - JVM 위에서 돌아가며 Java에 파이썬,루비 등의 특징을 얹었기 때문에 Java와 문법이 유사하다.
- Gradle wrapper를 이용하여 gradle이 설치되어있지 않아도 사용 가능(버전도 신경쓰지않아도 됨)
- 확장성이 뛰어나다.

<br><br>

## **Gradle vs Maven**
-  Gradle의 Groovy 언어를 이용한 스크립트 작성 vs Maven의 xml 작성
- Gradle의 주입 방식 vs Maven의 상속 방식
- Gradle이 Maven보다 빌드속도가 더 뛰어나다.

<br><br>

## **Gradle 설치**
### **Window 설치**
#### 1. Gradle 다운로드
- [Gradle 홈페이지](http://www.gradle.org/) 의 다운로드 페이지에서 gradle을 다운로드한다.
   - `binary 파일` (Gradle 파일만)이나 `complete 파일` (문서 등이 통합 된 파일) 아무거나 받는다.

#### 2. 환경설정
- `C:\Gradle` (권장 디렉토리)에 디렉토리를 만든다.
- `C:\Gradle` 디렉토리에 압축을 푼 gradle 폴더를 넣는다.
- 시스템 환경변수 새로만들기 
   - 변수이름 : GRADE_HOME 
   - 변수 값 : C:\Gradle\gradle-c
- 시스템 환경변수의 PATH 편집
   -  C:\Gradle\gradle-6.0.1\bin 추가
   - gradle-6.0.1 은 내가 다운로드 받은 6.0.1버전의 폴더명이며 각자의 폴더명에 따라 설정한다.
- CMD 에서 `gradle -v`를 입력하고 gradle 정보가 나오면 성공!
```cmd
> gradle -v
```
<br><br>

### **mac 설치**
#### 1. Gradle 다운로드
- Homebrew를 이용하여 설치
```mac
$ brew install gradle
```
- CMD 에서 `gradle --version`를 입력하고 gradle 정보가 나오면 성공!
```mac
-> gradle --version
```

<br><br>

## **Gradle 의존성 설명서(build.gradle)**
build.gradle 의 코드들을 살펴보면서 라이브러리 의존주입을 어떻게 사용해야 하는지 알아보자.
### dependency 의존 블록
- 라이브러리 의존성 설정하기
```gradle
dependencies{
   compile group: 'org.matamong', name: 'matamong-core', version: '0.0.1'

   //혹은 아래처럼 짧게 작성가능
   compile 'org.matamong:matamong-core:0.0.1'
}
```
<br><br>

- 라이브러리 스코프 설정

```gradle
dependencies {
   // 컴파일 시에 의존주입
   compile "org.matamong:matamong-core:0.0.1" 
   
   // 컴파일 시에만
   compileOnly "org.matamong:matamong-core:0.0.1"

   //실제 실행 시에 의존주입(compile을 포함)
   runtime "org.matamong:matamong-core:0.0.1"
   
   //실제 실행 시에만(compile을 포함)
   runtimeOnly "org.matamong:matamong-core:0.0.1"

   //실제 런타임시에 컨테이너로부터 제공받기 때문에 빌드 결과물에서는 필요없는 것을 의존주입
   providedCompile "org.matamong:matamong-core:0.0.1" 

   //테스트 컴파일 시에 의존주입
   testCompile "org.matamong:matamong-core:0.0.1"

   //테스트 실제 실행 시에 의존주입 (compile, runtime, testRuntime 모두 포함)
   testRuntime "org.matamong:matamong-core:0.0.1"
}

```
등등 여러가지가 있다.

- 라이브러리 의존 예외 두기
```gradle
dependencies{
   compile("org.matamong:matamong-core:0.0.1")
      exclude module: 'test'
       exclude group: 'groupId', module: 'artifactId'
}
```


* * *

[참고 블로그] <br>
<br>
**Gradle Dependencies**  <br>
https://kwonnam.pe.kr/wiki/gradle/dependencies <br>
권남 님의 블로그

