---
layout: post
title: Let's Encrypt - Certbot으로 수동갱신 중 No module named cryptography 에러
description: >
  Let's Encrypt - Certbot으로 수동갱신 중, **No module named cryptography* 경고문이 나올 때 해결방법
author: matamong
noindex: true
categories: [Error, Let's Encrypt, Certbot, Certbot-auto]
tags: [error, Let's Encrypt, Certbot, Certbot-auto]
comments: true
---

# **Let's Encrypt/Certbot-auto 수동갱신 에러**(ImportError: No module named cryptography)

## 발생

- **`Let's Encrypt`** 를 수동갱신 하기위해 `certbot-auto`를 이용하는 와중에 `ImportError: No module named cryptography` 라는 에러가 뜨면서 갱신에 실패하였다. <br>
그래서 `cryptography` 를 설치해주었다.

```linux
> pip install cryptography 
```

그럼에도 불구하고 계속 같은 에러가 발생했다.


## 원인
- `Let's Encrypt`의 파이썬 버전이 `site-packages`폴더에 있는 내용을 찾고있는데 그 폴더가 비어있어서 생기는 에러이다. <br> 
이상하게 그 내용들은 `site-packages`폴더가 아닌 `dist-packages`에 있는 모양이다.

## 해결
- python 폴더로 가서 내용을 다시 정리해준다.
```linux
> pip install cryptography (이미 인스톨 했다면 넘어가면 됨) 
> cd /opt/eff.org/certbot/venv/lib64/python2.7 
> mv site-packages site-packages.sav 
> ln -s dist-packages/ site-packages 
```
그리고 다시 갱신해주면 잘 된다.

- 참고로 수동갱신할 때 pip 버전을 업그레이드하라는 경고가 나온다. 이 경우엔 cryptography만 제대로 설치되었으면 무시해도 된다고 한다.

* * *
## 환경
AWS EC2 `Amazon Linux AMI 2018.03.0` <br>
python2.7
## 출처
[Stack Overflow: How to fix ImportError: No module named cryptography?
](https://stackoverflow.com/questions/57891591/how-to-fix-importerror-no-module-named-cryptography)