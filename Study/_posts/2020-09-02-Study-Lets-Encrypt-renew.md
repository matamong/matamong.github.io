---
layout: post
title: Let's Encrypt 인증서 수동 갱신하는 법
image: https://letsencrypt.org/images/letsencrypt-logo-horizontal.svg
description: >
  Let's Encrypt SSL 인증서를 Certbot을 이용하여 수동갱신 해보자.
author: matamong
noindex: true
categories: [Study, SSL, Let's Encrypt, Certbot]
tags: [study, SSL, Let's Encrypt, Certbot
comments: true
---

# Let's Encrypt 수동갱신

Let's Encrypt에서 SSL인증서를 발급 받으면 90일에 한 번 새롭게 갱신을 해줘야 한다. <br>
만료되기 2주전에 등록된 이메일로 만료 이메일이 올 것이므로 메일을 받았다면 갱신을 해보자. <br>
SSL 인증서를 발급받을 때 `Certbot`이라는 툴을 이용하였듯이 **갱신할 때도 편리하게 `Certbot`을 이용하면 된다.** <br>
(물론 수동 갱신이 귀찮다면 자동갱신으로 설정할 수 있다.) <br>

<br>

# 갱신 방법
- `certbot-auto` 툴이 있는 곳을 찾아 `renew`를 해준다. 이 때 처음 돌리는 것이니 **`Dry Run`** 을 해준다.
    - **`Dry Run`** : 모의 테스트, 예행 연습의 뜻을 가졌으며 실제로 실행시키기 전에 시험 삼아 시뮬레이션 해보는 옵션
```Linux
[certbot-auto가 있는 디렉토리]>$ ./certbot renew --dry-run
```
성공적으로 갱신된다면 다음과 같은 문구가 뜬다.
```Linux
Congratulations, all renewals succeeded. The following certs have been renewed: pem키가 있는 디렉토리 (success)
Dry Run : simulating 'certbot renew' close to cert expiry(The test certificates above have not been saved.)
```
대충, 성공했고 pem키는 이 디렉토리에 있다 정도를 알려주는 내용이다.<br>

[만약, ImportError: No module named cryptography 에러가 나온다면?](https://github.com/matamong/Study/blob/master/Error_Solution/Let'sEncryptRenew.md)

<br><br>

- Dry run을 해줫으니 이제 진짜 `renew`를 해준다.
```Linux
[certbot-auto가 있는 디렉토리]>$ ./certbot renew
```
성공적으로 갱신된다면 다음과 같은 문구가 뜬다.
```Linux
Congratulations, all renewals succeeded. The following certs have been renewed: pem키가 있는 디렉토리 (success)
```



