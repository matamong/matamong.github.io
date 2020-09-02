---
layout: post
title: ORA-022910-parent key not found 에러
description: >
  Oralce 부모키를 찾을 수 없는 에러
author: matamong
noindex: true
categories: [Error, Oracle, Maven, DBMS]
tags: [error, Oracle, Maven, DBMS]
comments: true
---

# **ORA-022910: integrity constraint (***) viloated - parent key not found**

## 증상
테이블에 `insert`하려는데 해당 에러가 나타났다.
## 원인
자식테이블에서 부모테이블을 참조해서 `insert` 하고 있는데 별안간 부모키가 없어졌을 때 이 에러가 나타난다.
## 해결
- 부모테이블에 있는 키를 이용해 자식테이블에 `insert`한다.
- `insert` 하려고 했던 정보를 미리 부모테이블에 `insert` 하여 **존재하게한다**.