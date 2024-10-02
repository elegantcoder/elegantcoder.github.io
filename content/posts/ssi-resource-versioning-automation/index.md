---
title: "SSI 를 활용한 프론트엔드 리소스 버저닝"
author: elegantcoder
date: 2013-02-06T00:44:59
publishDate: 2013-02-06T00:44:59
summary: "## 개요
리소스 버저닝은 리소스 캐시를 목적으로 한다.
리소스 캐시는 해당 파일이 변경된 경우 컨텐츠를 새로 전송(Cache Miss)하고, 변경되지 않았다면 컨텐츠를 전송하지 않는(Cache Hit)다.
많이 사용되는 방법으로 버전이름을 쿼리스트링(Query String, GET 파라미터라고도 함)에 명시하는 방법이 있다.
버전이름을 쿼리스트링에 명시하는 방법은"
url: "/posts/ssi-resource-versioning-automation"
aliases: ["/ssi-resource-versioning-automation"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:28
isCJKLanguage: true
categories:
  - front-end
tags:
  - Front-end
  - outdated
params:
  images:
    - "undefined"
---
개요
--

리소스 버저닝은 리소스 캐시를 목적으로 한다. 리소스 캐시는 해당 파일이 변경된 경우 컨텐츠를 새로 전송(Cache Miss)하고, 변경되지 않았다면 컨텐츠를 전송하지 않는(Cache Hit)다.

많이 사용되는 방법으로 버전이름을 쿼리스트링(Query String, GET 파라미터라고도 함)에 명시하는 방법이 있다.

버전이름을 쿼리스트링에 명시하는 방법은

```
<script src="vendors.js?v0.9.5"></script>
<script src="app.js?v0.9.5"></script>   
```

이와 같이 버전이름(v0.9.5)을 파라미터로 적어주고, 이후 버전이 올라가면 버전이름을 변경시켜주는 방법이다.

```
<script src="vendors.js?v0.9.6"></script>
<script src="app.js?v0.9.6"></script>
```

이 방법은 파일 이름을 변경하지는 않지만 파일의 내용이 변경될 때마다 주소를 변경시키므로 브라우저나 웹서버 간에 혹시 발생할 수 있는 **캐시가 먹어 파일이 갱신되지 않는 문제**를 방지할 수 있다.

일괄 버전 업데이트? 새로 업데이트된 파일만 버전 업데이트?
---------------------------------

하지만 위 방식에도 맹점이 있다. 만약 0.9.6 업데이트가 app.js 에만 관련되어 있다면 vendors.js 는 굳이 새로 전송할 필요가 없는데도 다시 전송하게 되므로, 버전을 모두 변경하는 방식은 약간의 손해를 감수해야 한다.

현재 Baas.io 프로젝트에서는 1. 빌드 프로세스에 버전명을 명시하기 까다롭고, 2. 잘 변경되지 않는 라이브러리파일과 자주 변경되는 어플리케이션 파일을 구분하고 있어서 **새로 업데이트된 파일만 버전을 올려주는 방법**을 적용했다.

자동 버저닝
------

### SSI 코드

```
<!--#config timefmt="%s" --> <!-- 시간을 읽는 방법을 Unix Timestamp 로 설정 -->
<link href="/css/styles.css?<!--#flastmod virtual="/css/styles.css" -->" rel="stylesheet" type="text/css" /> <!-- last modified 타임을 GET 파라미터로 넘김 -->
<script type="text/javascript" src="/js/vendor/vendors.min.js?<!--#flastmod virtual="/js/vendor/vendors.min.js" -->"></script>
<script type="text/javascript" src="/js/app/common.min.js?<!--#flastmod virtual="/js/app/common.min.js" -->"></script>
```

### 결과

```
<link href="/css/styles.css?1355283501" rel="stylesheet" type="text/css" />
<script type="text/javascript" src="/js/vendor/vendors.min.js?1359420532"></script>
<script type="text/javascript" src="/js/app/common.min.js?1360469267"></script>
```

새로 업데이트된 파일의 버전을 올려주는 방법을 자동화하기 위해 파일의 각 마지막 변경시간(Last Modified Time) 을 Unix 타임스탬프형식으로 쿼리스트링으로 넘겨주도록 했다.

이렇게 구성하면 각 파일의 마지막 변경시간(서버에 탑재된 시간)이 이 파일의 버전이 되므로 선택적으로 버저닝이 가능하다. 그리고 마지막 변경시간을 SSI 를 통해 구하도록 되어 있어서 수동으로 버전을 올려줄 필요가 없다.

장점
--

-   버저닝 최적화/자동화
    -   릴리즈 주기에 맞춰 버전을 일괄변경 한 것이 아니라 각 파일마다 버저닝했으므로 업데이트 되지않은 파일들은 이전 캐시를 그대로 사용할 수 있다.
-   간단한 적용
    -   빌드 과정에 손을 쓰지 않고 SSI 코드를 삽입한 것이라 간단하게 적용할 수 있었다.

단점
--

-   HTTP Request당 오버헤드 증가
    -   파일의 마지막 변경시간을 구해야 하므로 Disk IO 발생이 늘어나게 된다.

P.S.
----

squid 등 일반적으로 많이 사용되는 프록시 서버에서는 쿼리스트링이 달려있는 리소스들은 캐시를 타지 않고 모두 새로 받아온다고 한다. 이런 이유로 쿼리스트링으로 버저닝하는 것보다 파일명 자체를 변경시키는 것이 더 낫다고 한다. [Revving Filenames: don’t use querystring](http://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/)
