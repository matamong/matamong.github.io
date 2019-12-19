---
layout: post
title: Your local changes to the following files would be overwritten by merge
description: >
  **git pull** 을 하려고 하는데 Your local changes to the following files would be overwritten by merge 라는 경고문이 나와서 해결해 보았다.
author: matamong
noindex: true
categories: [Error, git]
tags: [error, git]
---

# **git pull 할 때 Your local changes to the following files would be overwritten by merge 에러**

**`git pull`** 을 하려고 했는데 <br>
**`Your local changes to the following files would be overwritten by merge:`** <br>
라고 뜨며 **`pull`** 이 되지 않았다. <br><br>

## **원인**
로컬의 소스가 제대로 처리되지않아서 리모트의 소스를 **`pull`** 할 수 없어서 일어나는 에러였다. <br>


## **해결**
로컬의 소스를 임시저장하는 **`stash`** 를 이용하여 꼬인 부분을 임시저장하고 일단 **`pull`** 했다.
<br><br>

<br><br>

**`stash`란?**
- 현재 **unstaged**인 파일들을 임시저장하고 **HEAD**의 상태로 백업하는 명령어
     - **`$git stash -list`** : stash로 백업된 **list**를 보여준다.
     - **`$git stash pop`**  : stash로 백업된 파일을 **다시 적용**한다.
<br><br>

```git
$ git statsh
$ git pull

$ git stash pop
```