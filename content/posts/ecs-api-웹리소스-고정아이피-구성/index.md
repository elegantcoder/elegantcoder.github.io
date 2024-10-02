---
title: "ECS – API애플리케이션 – S3 웹리소스 고정아이피 구성"
author: elegantcoder
date: 2020-06-30T22:28:02
publishDate: 2020-06-30T22:28:02
summary: "ECS로 구성된 애플리케이션의 웹 리소스의 고정아이피 구성이 필요했다. 가끔 유용하게 활용할 수 있는 사례라 구성도를 따로 그려봤다. 문제정의 애플리케이션 서버는 ECS &#8211; Fargate &#8211; ALB 환경으로 구축 S3에 웹 리소스가 배포됨. 애플리케이션 서버와 웹리소스에 고정아이피 주소 필요 백엔드와의 연결은 HTTP만 사용해도 됨. 해결 Network Load Balancer에 고정아이피를 설정 NLB는 포트라우팅만 지원하므로 Nginx의 virtual server 설정으로 [&hellip;]
"
url: "/posts/ecs-api-웹리소스-고정아이피-구성"
aliases: ["/ecs-api-웹리소스-고정아이피-구성"]
titleImage: "undefined"
draft: false
lastmod: 2020-07-01T01:13:47
isCJKLanguage: true
categories:
  - aws
tags:

  - outdated
params:
  images:
    - "undefined"
---
ECS로 구성된 애플리케이션의 웹 리소스의 고정아이피 구성이 필요했다. 가끔 유용하게 활용할 수 있는 사례라 구성도를 따로 그려봤다.

문제정의
----

-   애플리케이션 서버는 ECS – Fargate – ALB 환경으로 구축
-   S3에 웹 리소스가 배포됨.
-   애플리케이션 서버와 웹리소스에 고정아이피 주소 필요
-   백엔드와의 연결은 HTTP만 사용해도 됨.

해결
--

-   Network Load Balancer에 고정아이피를 설정
-   NLB는 포트라우팅만 지원하므로 Nginx의 virtual server 설정으로 애플리케이션과 웹리소스 라우팅
-   Task Definition상 Nginx와 애플리케이션 서버를 하나에 둠
-   Nginx는 리버스프록시(localhost)로 애플리케이션 연결, S3와의 연결도 리모트 리버스프록시 사용.
-   S3는 Bucket Policy를 이용해 VPC고정으로 연결

구성도
---

<figure><img src="ECS구성.png" alt=""></figure>

참고
--

-   ALB + Global Accelarator조합은 배제했다. 국내용 서비스이고 한국OUT – 한국IN의 비용이 비싼것으로 판단했다.
-   ECS Fargate는 Network Mode 중 AWSVPC모드만 지원한다. AWSVPC는 각 태스크마다 ENI(Elastic Network Interface)를 제공하고 Private IP와 Public IP를 제공한다. 동일 태스크 내 다른 컨테이너와의 통신에는 localhost 와 포트번호를 사용한다.
-   NLB에 TLS구성은 2019년 1월부터 지원했다([https://aws.amazon.com/about-aws/whats-new/2019/01/network-load-balancer-now-supports-tls-termination/](https://aws.amazon.com/about-aws/whats-new/2019/01/network-load-balancer-now-supports-tls-termination/))
-   NLB와 Nginx라우팅 대신, ALB를 함께 사용하는 방법으로 Internet facing NLB + Internal ALB + Lambda를 활용하는 방법이 있다. ([https://aws.amazon.com/blogs/networking-and-content-delivery/using-static-ip-addresses-for-application-load-balancers/](https://aws.amazon.com/blogs/networking-and-content-delivery/using-static-ip-addresses-for-application-load-balancers/))
