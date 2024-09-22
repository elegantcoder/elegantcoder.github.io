---
title: "Facebook Comment 를 Disqus 로 내보내는 FB2Disqus"
author: elegantcoder
date: 2013-02-24T15:21:59
publishDate: 2013-02-24T15:21:59
summary: "페이스북의 Social comment 를 잘 사용하고 있었지만, Disqus 로 옮기고 싶었다.
페이스북 사용자가 아니면 댓글을 달기 어려운 면이 있기 때문에 좀 더 확장하고 싶었다. 그리고 더 큰 이유는 디자인 이었다.
블로그를 새로 꾸미면서 블로그 글 영역을 가변폭으로 만들었는데 Facebook 의 social comment 는 고정폭 만 지원하고 있다.
"
url: "/posts/fb2disqus"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:25
isCJKLanguage: true
categories:
  - etc
tags:
  - etc
  - outdated
params:
  images:
    - "undefined"
---
페이스북의 Social comment 를 잘 사용하고 있었지만, Disqus 로 옮기고 싶었다.

페이스북 사용자가 아니면 댓글을 달기 어려운 면이 있기 때문에 좀 더 확장하고 싶었다. 그리고 더 큰 이유는 디자인 이었다.

블로그를 새로 꾸미면서 블로그 글 영역을 가변폭으로 만들었는데 Facebook 의 social comment 는 고정폭 만 지원하고 있다. 그리고 페이스북의 댓글창보다는 Disqus 의 디자인이 더 아름다워 보이고 내 블로그에 더 잘 어울린다는 생각이 들었다.

Exporter 쯤은 있겠지 생각만 하고 있다 검색해보니 나오질 않아서 하나 만들게 되었다.

우선은 요즘 주로 사용하고 있는 Coffeescript 로 만들려고 했다. 마이그레이션은 필요한 데이터를 모으고 재구성해서 다시 API를 호출해야 하는 과정인데, 이 과정을 비동기식으로 작성하려니 괜히 헷갈리고 코드 모양은 안나니 기분이 좀 상했다. 그 덕에 Promise 공부만 더 하게 됐다.

그래서 오랫만에 PHP로 작업했다. 두 서비스 모두 PHP SDK 를 지원하고 있어서 어렵지 않게 구현할 수 있었다.

Disqus 에서 작성자의 프로필 사진을 강제로 넣을 수가 없어 프로필 사진은 이전하지 못했지만 우선은 만족. Github 에 올렸다. [FB2Disqus on Github](https://github.com/elegantcoder/FB2Disqus/)

테스트가 없는 것과 FQL 을 쓰면 좀 더 나을 뻔 했는데, 뭐 그건 문제가 생기면 작성하기로 하고..

그나저나 댓글창도 바뀌었는데 댓글 하나 달아주지 않으시렵니까? 🙂

Facebook social comment export to Disqus


==========================================

[FB2Disqus on Github](https://github.com/elegantcoder/FB2Disqus/)
=================================================================