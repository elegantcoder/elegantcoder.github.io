---
title: "Docker Entrypoint와 쉘 함수의 사용"
author: elegantcoder
date: 2020-04-30T14:02:18
publishDate: 2020-04-30T14:02:18
summary: "Docker 컨테이너는 Entrypoint를 가지고 있어서, 컨테이너를 하나의 실행파일처럼 동작할 수 있게 해준다. An ENTRYPOINT allows you to configure a container that will run as an executable. https://docs.docker.com/engine/reference/builder/#entrypoint 이를 이용해 때때로 활용하는 기법이 있는데, Entrypoint를 쉘 함수로 엮어 사용하는 방법이다. 최근 Jenkins에 AWSCLI v2를 사용해야할 일이 있었다. 이미 aws v1이 시스템 전체에 설치되어 운영되고 있기 때문에 [&hellip;]
"
url: "/posts/docker-entrypoint와-쉘-함수의-사용"
aliases: ["/docker-entrypoint와-쉘-함수의-사용"]
titleImage: "undefined"
draft: false
lastmod: 2020-05-11T23:45:09
isCJKLanguage: true
categories:

tags:
  - docker
  - TIL
  - outdated
params:
  images:
    - "undefined"
---
Docker 컨테이너는 Entrypoint를 가지고 있어서, 컨테이너를 하나의 실행파일처럼 동작할 수 있게 해준다.

> An `ENTRYPOINT` allows you to configure a container that will run as an executable.
> 
> https://docs.docker.com/engine/reference/builder/#entrypoint

이를 이용해 때때로 활용하는 기법이 있는데, Entrypoint를 쉘 함수로 엮어 사용하는 방법이다.

최근 Jenkins에 AWSCLI v2를 사용해야할 일이 있었다. 이미 aws v1이 시스템 전체에 설치되어 운영되고 있기 때문에 명령어 충돌을 피하기 위해 이 방법을 사용했다.

```
#!/bin/env bash -x 

docker pull amazon/aws-cli
aws () { docker run --rm amazon/aws-cli $@ }
```

참고로 Interactive Shell에서는 alias를 사용해도 되지만, Non-Interactive 쉘에서 alias를 사용하려면 shopt 설정이 필요하다.
