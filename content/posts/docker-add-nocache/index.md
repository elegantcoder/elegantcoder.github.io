---
title: "Dockerfile – ADD 를 통해 build 캐시막기"
author: elegantcoder
date: 2014-08-31T10:07:39
publishDate: 2014-08-31T10:07:39
summary: "Dockerfile 을 수정하고 다시 빌드할 때 cache 덕분에 빠르게 재실행이 가능하다.
Dockerfile 의 명령라인이 변경되지 않으면 자동으로 캐시가 작동하게 되는데 가끔 이 기능이 불편할 때가 있다.
Node 개발용 서버를 구축하는 중에 이런 경우를 만났다.
&#8220;`git pull&#8220;` 을 하고, 의존성 해결을 위해 &#8220;`npm install&#8220;` 을 하는 플로우인데, &#8220;`RUN git pull&#8220;` 행이 변경되지 않으니 계속 캐시가 작동했다.
캐시를 작동하지 않게 하는 명령을 추가하는 것에 대해 [이 곳에서](https://github.com/docker/docker/issues/1996) 계속 논의가 진행중인데, 이 이슈를 살펴보다 랜덤스트링을 추가하는 괜찮은 방법을 알게
"
url: "/posts/docker-add-nocache"
aliases: ["/docker-add-nocache"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:12:24
isCJKLanguage: true
categories:
  - dev
tags:
  - Dev.
  - outdated
params:
  images:
    - "undefined"
---
Dockerfile 을 수정하고 다시 빌드할 때 cache 덕분에 빠르게 재실행이 가능하다.

Dockerfile 의 명령라인이 변경되지 않으면 자동으로 캐시가 작동하게 되는데 가끔 이 기능이 불편할 때가 있다.

Node 개발용 서버를 구축하는 중에 이런 경우를 만났다.

`git pull` 을 하고, 의존성 해결을 위해 `npm install` 을 하는 플로우인데, `RUN git pull` 행이 변경되지 않으니 계속 캐시가 작동했다.

캐시를 작동하지 않게 하는 명령을 추가하는 것에 대해 [이 곳에서](https://github.com/docker/docker/issues/1996) 계속 논의가 진행중인데, 이 이슈를 살펴보다 랜덤스트링을 추가하는 괜찮은 방법을 알게되었다.

`ADD http://www.random.org/strings/?num=10&len=8&digits=on&upperalpha=on&loweralpha=on&unique=on&format=plain&rnd=new /var/tmp/uuid`

이 명령을 더이상 캐시가 필요하지 않는 곳에 추가해주면 된다. 이렇게 추가해주면 build 할 때마다 랜덤스트링을 다운받고 컨테이너의 파일이 변경되기 때문에 이후의 명령들은 캐시가 동작하지 않게된다.
