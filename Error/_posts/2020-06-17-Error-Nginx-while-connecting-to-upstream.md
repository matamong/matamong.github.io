---
layout: post
title: Nginx while connecting to upstream 에러
image: https://www.nginx.com/wp-content/uploads/2018/08/NGINX-logo-rgb-large.png
description: >
  Nginx 배포 중 별안간 Nginx의 5xx에러가 났을 때
author: matamong
noindex: true
categories: [Error, Nginx]
tags: [error, Nginx]
comments: true
---

# **while connecting to upstream**


## 발단
- 기존 home.html 에 9.83KB 짜리 이미지가 있었는데 추가로 39.8KB, 31.6KB 크기의 이미지를 추가해서 배포
- 배포 후, 추천코드를 입력하던 사용자가 갑자기 에러가 났다고 신고
- 들어가보니 Nginx의 크고 아름다운 5xx 에러가 장악
- 서버로그처리를 안해놔서 멘붕(반성)

## 의심
- Nginx 에러이니 Nginx 에러로그를 파헤쳐봄 (웹 서비스 로그는 특별한게 없었음)
    ```cmd
    
    sudo vim /etc/nginx/nginx.conf <--엔진엑스 설정파일에서 에러로그파일 위치를 확인

    sudo vim /var/log/nginx/error.log 
    ```

- 처음 에러가 시작된 곳의 로그

```cmd
번호 : 번호  upstream prematurely closed connection while reading response header from upstream, client:클라이언트 IP, server: 내 서버, request: 추천코드 검증 API", upstream: "API URL", host: "내 서버", referrer: "페이지주소"

connect() failed (111: Connection refused) while connecting to upstream
connect() failed (111: Connection refused) while connecting to upstream
connect() failed (111: Connection refused) while connecting to upstream
connect() failed (111: Connection refused) while connecting to upstream
connect() failed (111: Connection refused) while connecting to upstream
...반복
```

- `upstream prematurely closed connection while reading response header from upstream`
    - 리스폰스 헤더와 연결하고 읽던 도중에 끊겨부렀고 그 후로 계속 연결에 실패하면서 5xx 에러를 뿜어대고 있었다.

- 구글링해보니 이러한 연결 실패는 대부분 `timeout`문제라고 했다. 충분히 연결하여 읽을 시간을 주지않기때문에 연결에 실패하게 되고 엔드포인트에서는 어쩔 수 없이 빈값을 전해주기도 한다는 것.
- 새롭게 배포한 이미지를 Nginx서버에서 끙끙대면서 읽으려다가 실패한 것으로 유츄했다.

## 해결??
- timeout 문제를 해결하면 되었다.
- Nginx 서버 설정파일의 http 부분에서 버퍼와 타임아웃값을 손본다.
```log
location / {
    proxy_connect_timeout 300s;
    proxy_read_timeout 600s;
    proxy_send_timeout 600s;
    proxy_buffers 8 16k;
    proxy_buffer_size 32k;
}

// 사실 300s 했다가 간헐적으로 에러가 나서 600s로 올림
```


- Nginx 설정파일에 문제가 없는지 확인
```cmd
nginx -t
```
- Nginx 리로드
```cmd
sudo service nginx reload
```
- 고친 후 Travis CI빌드다시하고 배포!

    **그러나**

<br>

**배포할 때는 괜찮다가 사용자가 살짝만 몰려도 같은 페이지에서 에러가 간헐적으로 일어났음**

## 다시 의심

- 타임아웃을 늘려도 커넥션이 안 될 정도로 해당 페이지가 정보를 읽어들이기 벅차다는 뜻이라고 해석
- 해당 페이지에서 외부링크를 불러올 때 시간이 너무 걸리는게 아닐까 생각
    - 프론트 ui를 외부 css인 fomantic-ui로 대체하고 있었기 때문에 css,js 를 불러오는 cdn이 있었음.
    - 이미지를 깃헙저장소에 올린 뒤 그 이미지의 저장소 주소를 끌어오고 있었음
    - cdn의 서버가 거의 해외에 있고, 깃헙저장소의 서버도 해외에 있을거라 예상..
    - 구글링으로 cdn에 ssl까지 더해지면 시간이 너무나 느려진다는 것을 발견.(떡칠하지 말라하더라...^-ㅠ)
- favico.ico 가 spring security에 걸려서 403(접근제한)을 띄움. 하지만 브라우저에서는 안불러와지는 favico.ico를 있는 힘껏 불러오려고 하고 있었음.



## 해결


- 네트워킹을 경량화해서 응답시간을 빠르게 해봄
    - 해외 깃허브주소로 이미지를 불러왔었는데 이미지 용량을 줄인 뒤 직접 서버에서 제공하기로 함
    - Jquery도 직접 서버에서 제공하기로 함
    - 나눔바른고딕 폰트가 엄청 무거웠는데 경량화된 버전을 끌고옴
    - 구글폰트도 해외cdn으로 무거운거 불러오는것보다 직접 뿌림

- 배포 후, 며칠 지켜보니 페이지 일일 사용자 수가 비슷한 상황에서 더 이상 에러가 나오지 않음

## 결론

- Nginx의 502 에러는 원인이 너무나 다양하기때문에 로그를 잘 읽고 끈질기게 원인을 추적해야할것같다.
- 앞으로는 TTTB(Time To Talk Befriending)도 신경을 많이 써야겠다.

* * *
[참고자료]

<br>

https://stackoverflow.com/questions/36488688/nginx-upstream-prematurely-closed-connection-while-reading-response-header-from

https://github.com/owncloud/client/issues/5706

https://discuss.kubernetes.io/t/upstream-prematurely-closed-connection-while-reading-response-header-from-upstream/4396

https://m.blog.naver.com/myaddressis/221657722723