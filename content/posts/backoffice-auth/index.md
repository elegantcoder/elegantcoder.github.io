---
title: "어느 스타트업의 백오피스 인증 구현기"
author: elegantcoder
date: 2014-07-27T15:52:24
publishDate: 2014-07-27T15:52:24
summary: "![백오피스 로그인 화면](/sites/default/files/backoffice_login.png &#8220;백오피스 로그인 화면&#8221;)
스타트업에는 놓치지 않아야 할 여러 가지가 있다. 난 그 중 백오피스를 꽤 높은 순위에 둔다.
백오피스를 가졌을 때의 장점은
1. 서비스를 만들고 지표의 변화를 감지해 서비스를 개선하는 데 큰 도움이 된다.
2.
"
url: "/posts/backoffice-auth"
aliases: ["/backoffice-auth"]
titleImage: "backoffice_login.png"
draft: false
lastmod: 2015-07-24T04:12:30
isCJKLanguage: true
categories:
  - dev
tags:
  - Dev.
  - outdated
params:
  images:
    - "backoffice_login.png"
---
스타트업에는 놓치지 않아야 할 여러 가지가 있다. 난 그 중 백오피스를 꽤 높은 순위에 둔다.

백오피스를 가졌을 때의 장점은

1.  서비스를 만들고 지표의 변화를 감지해 서비스를 개선하는 데 큰 도움이 된다.
2.  개발자전용 인터페이스(REST API 도구라든지 `<form>` 만 있는 HTML 페이지 등)를 사용하는 것보다 모두의 도구를 만드는 것이 가장 시간이 부족할 때 큰 도움이 된다.

지표에 대해 조금 더 이야기하면 IT 기업에서 지표를 측정하려면 개발자의 도움이 꼭 필요하다. Google Analytics와 같은 도구의 코드를 삽입하거나 데이터베이스 내의 수치 등의 많은 지표가 개발자의 손을 통해 측정되고 알 수 있게 되기 때문이다. 백오피스가 없는 경우 데이터에 접근성이 떨어지고 알고 싶은 리포트를 수동으로 작성할 수밖에 없다. 가뜩이나 인원도 시간도 모자란 스타트업에게 보고서 작성하는 시간을 할애하는 것과 같다고 생각한다.

현재 근무하고 있는 회사는 [KStyleTrip.com](http://kstyletrip.com)이고 첫 서비스로 [Luck2share](http://luck2share.com) 를 만들었다. 새로 입주한 사무실을 꾸미면서 위시리스트를 작성하려고 했는데 공수도 많이 들지 않을 테니 서비스로 만들어보자. 그리고 dogfooding 해보자는 개념으로 시작한 서비스다. [KStyleTrip 의 위시리스트 바로 가기](http://luck2share.com/#/DM58X9dperCrQDA8kJmP)

아직 kstyletrip.com 은 제작 중이기 때문에 백오피스의 플랫폼을 만드는 작업은 주로 이 Luck2share 를 대상으로 했다. 그리고 회사의 서비스와 관리자에게 필요한 기능은 앞으로 계속 늘어날 것이기 때문에 서비스마다 각기 다른 백오피스를 갖는 것이 아니라 모든 서비스가 통합된 백오피스를 제작하는 것으로 방향을 잡았다. 이 방법이 각 서비스가 방치되지 않고 잘 관리되는 방법이라는 것에는 이견의 여지가 없으리라고 생각한다.

DB 쿼리 vs. REST API
==================

여기까지 생각이 미치고 구현에 앞서 고민했던 점은 Luck2share 의 데이터를 가져오는 방법이었다. 검토했던 방법은 DB 포트를 열고 백오피스 서버에서 접근해 관리자 전용 API를 구현하는 것과 Luck2share 서버에 관리자용 API를 만들어 사용하는 것이었다.

Luck2share 의 DB 포트는 외부에 공개되지 않고 서버 안쪽에서만 사용하고 있다. 따라서 상대적으로 안전하게 보관되고 있는데 이 포트를 열어 보안이슈를 만들고 싶지 않았다. 그리고 이미 Luck2share 내에 DB쿼리하는 코드베이스가 충분한데 비슷한 코드를 다시 만들어야 하는 것에도 거부감이 있었다.

따라서 API는 Luck2share 서버에 두고 데이터를 가져오는 것으로 방향을 잡았다.

Private API 구축 – 비밀번호 공유 vs. 비대칭 키암호화
=====================================

두 번째 고민은 Private API를 구축하는 부분이었다. 관리자를 위한 API라면 서버 간, 또는 서버와 클라이언트 간 HTTP 통신을 구축하는데 인증수단이 반드시 있어야 한다. 따라서 HTTP의 Authorization헤더를 통해 인증하는 방법을 사용했다.

우리는 구글 앱스를 사용하고 있기 때문에 회원을 관리할 필요는 없다. OAuth 로그인을 정상적으로 마친 후 OAuth 로부터 얻어온 회원정보에서 몇 가지를 추려 다시 토큰을 만들었다.

이 토큰을 Luck2share 서버에서 검사하도록 해야 했는데 secret을 공유해 암호화/복호화하는 방식은 깔끔하지 않을 뿐 아니라 유출되었을 때 다른 서비스도 동시에 유출되기 때문에 안전하지 않다. 따라서 비밀 키/공개 키를 통한 암호화 방식을 선택했다.

아래와 같은 절차로 Private API에 접근할 수 있다.

1.  우선 비밀 키와 공개 키를 생성한다. 이 키 쌍의 비밀 키는 백오피스 서버에, 공개 키는 Luck2share(다른 서비스들의 서버)에 둔다
2.  사용자가 로그인할 때 사용자의 필수정보와 토큰 생성시간과 관련한 데이터를 묶는다. 시간을 묶으면 생성할 때마다 토큰이 확연히 달라지기 때문에 유출의 위험이 적다.
3.  2에서 만들어진 데이터를 백오피스 서버의 비밀 키로 암호화해 토큰으로 만든다.
4.  통신이 필요할 때 토큰을 Luck2share 에 전달한다.
5.  Luck2share 에서는 미리 받아놓은 공개 키로 토큰을 복호화한다.
6.  복호화가 정상적으로 이루어지고 조건에 맞으면 API 호출결과를 돌려주고 그렇지 않으면 401 Unauthorized를 내보낸다.

백오피스 API 서버에서 사용한 프레임워크는 [Restify](http://mcavage.me/node-restify/) 로, Node 의 [jwt](https://www.npmjs.org/package/jwt) 모듈과 [express-jwt](https://www.npmjs.org/package/express-jwt) 미들웨어를 사용해 큰 어려움 없이 Private API를 구축할 수 있었다.

(사실 키 암호화에 대해 깊은 조예가 있어서 떠올린 것이 아니라, jwt 모듈의 기능을 살펴보다 알게 되었다. :-P)

OAuth Login 과 만료시간
==================

앞서 밝혔듯 KStyletrip.com은 Google Apps for Business를 사용하고 있다. 따라서 구글에서 제공하는 OAuth 인증을 사용하면 되므로 회원가입, 회원정보 수정과 같은 것을 구현할 필요가 없었다. 단지 OAuth Consumer로서 회원정보를 활용하는 방법을 구현하면 되었다. 다만 Google에 로그인할 때 제공하는 Access Token의 만료시간은 1시간으로 굉장히 짧아서 API 호출 때마다 이 정보에 의존할 수는 없었다.

따라서 별도의 토큰을 만들어 유지하기로 했다. 이 토큰은 위에 설명한 Private API에 접근하는 토큰과 같은 것이다. 이 토큰을 만들고 사용하는데 만료기간이 너무 짧으면 불편할 것이고 너무 길면 퇴사하거나 변동이 생긴 직원에 대해 대처하기 어려워질 것으로 판단했다.

그래서 아래와 같은 방법으로 구현했다.

OAuth 인증 시에 오프라인 접근권한을 받을 수 있도록 했다. [오프라인 접근권한을 받으면 Refresh Token을 얻어올 수 있다](https://developers.google.com/accounts/docs/OAuth2WebServer). 이렇게 받아온 Refresh Token으로 백오피스 API 서버가 주기적으로 Google의 OAuth 서버에서 Access Token을 받아오도록 했다. 이렇게 하면 자동으로 사용자의 유효성을 측정할 수 있게 되어 퇴사한 직원에 대해 걱정하지 않아도 된다.

그리고 유효성이 확인된 토큰들을 메모리에 (사실은 API 서버의 변수에) 넣어두고 최초 로그인시 발급된 토큰을 갱신할 수 있는 API 를 만들었다. 변수에 저장된 토큰들은 서버를 새로 시작하면 날아가 버리기 때문에 다른 방법으로 보완할 예정이다. 2014. 8. 3 추가: [nedb](https://www.npmjs.org/package/nedb) (Node Embeded DataBase) 를 사용했다.

프론트엔드에서는 주기적으로 갱신 API를 호출해 토큰을 갱신하는 동시에 자체토큰을 가진 사람이 Google에서도 유효하게 판단하는지를 확인했다.

후기
==

일주일 남짓한 시간에 우리는 꽤 안정적인 백오피스 플랫폼을 갖게 되었다. 그리고 이제 실제로 비즈니스에 필요한 API를 필요할 때마다 개발하기만 하면 된다.

참고
==

-   [KStyletrip,com](http://kstyletrip.com)
-   [Luck2share.com](http://luck2share.com)
-   [Restify](http://mcavage.me/node-restify/)
-   [jwt](https://www.npmjs.org/package/jwt)
-   [express-jwt](https://www.npmjs.org/package/express-jwt)
-   [Using OAuth 2.0 for Web Server Applications](https://developers.google.com/accounts/docs/OAuth2WebServer)
