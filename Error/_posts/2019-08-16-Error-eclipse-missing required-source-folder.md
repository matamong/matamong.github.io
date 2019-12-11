---
layout: post
title: eclipse-missing-required source folder
description: >
  **missing required source folder** 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [IDE, eclipse]
tags: [error, IDE, eclipse]
---

# **missing required source folder**

eclipse에서 프로젝트를 불러오는 과정에서 <br>
`Project 'xxxx' is missing required source folder: 'src/main/resources'	Build path	Build Path Problem` <br>
이라는 문제가 생겼다.<br>

## 원인
알림에서도 알려주듯이 `Build Path` 문제이며 프로젝트를 불러올 때 새로운 환경에서 경로가 맞지않아 뜨는 문제이다. <br>

## 해결
- build Path를 확인한다. 
     - 프로젝트 우클릭 
     - property 로 진입
     - `Java Compiler`에서 Compiler 버전을 맞춰준다.
     - `Project Facets`에서 Dynamic WebModule , Java의 버전을 맞춰준다.

- missing 으로 표기되는 src폴더를 지운다.
     - 프로젝트 우클릭 
     - Build Path - Configure Build Path
     - source 탭
     - `missing` 표기 된 폴더를 선택하고 `remove`한다.


