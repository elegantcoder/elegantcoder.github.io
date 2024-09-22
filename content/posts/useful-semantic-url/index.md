---
title: "Semantic URL 를 프로젝트 내 소통수단으로!"
author: elegantcoder
date: 2011-12-13T14:48:08
publishDate: 2011-12-13T14:48:08
summary: "Semantic URL이란?
Semantic URL 은 Clean URL, Fancy URL, Rewritten URL 으로 불리기도 합니다. 위키피디아에서는&nbsp;Semantic URL과&nbsp;Clean URL을 각각 다른 용어로 정의하고 있기는 하지만 내용을 살펴보면 근본적으로는 비슷합니다.&nbsp;
즉, 비전문가(사용자)에게 친숙한, (의미를 표현하는)구조적인 URL입니다.&nbsp;
"
url: "/posts/useful-semantic-url"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:17:08
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
Semantic URL이란?
---------------

Semantic URL 은 Clean URL, Fancy URL, Rewritten URL 으로 불리기도 합니다. 위키피디아에서는 [Semantic URL](http://en.wikipedia.org/wiki/Semantic_URL)과 [Clean URL](http://en.wikipedia.org/wiki/Clean_URL)을 각각 다른 용어로 정의하고 있기는 하지만 내용을 살펴보면 근본적으로는 비슷합니다. 

즉, 비전문가(사용자)에게 친숙한, (의미를 표현하는)구조적인 URL입니다. 

Semantic URL 이 유행하기 시작한 것은 대략 업계에 웹 2.0, Semantic 이라는 키워드가 유행하기 시작할 때 쯤부터 였던 것 같습니다.

Semantic URL과 일반적인 URL의 차이는 한 눈에 드러납니다.  

**일반 URL**

**Semantic URL**

board.php?id=notice&mode=list&page=1

board/notice/list/1 

board.php?id=notice&mode=view&entry=10 

board/notice/10 

더 짧을 뿐 아니라 URL만 보고도 어떤 역할을 하는 페이지인지 쉽게 짐작할 수 있습니다.  

Semantic URL을 사용하면 

-   깔끔한 URL을 유지할 수 있습니다.
-   사용자가 URL을 기억하기 쉽습니다.
-   주요정보를 변수로 처리하지 않고 디렉토리 인 것처럼 다루므로 SEO 에도 도움이 됩니다. 

이 모든 장점은 Semantic URL 자체가 충분히 의미적일 때 가능합니다. 

여기에 프로젝트 관점에서의 Semantic URL의 활용을 생각해봅니다. 

1\. 어휘(Vocabulary)의 통일
----------------------

프로젝트마다 내부에서 통용되는 어휘들이 있습니다. 하지만 역할에 따라 비슷하지만 전혀 다른 어휘들을 사용하기도 합니다.

예를 들어 회원가입 페이지를 두고

-   기획자 : (PPT를 보며) “사용자(user)가 이 곳을 클릭해서 가입(sign up) 합니다.”
-   프론트엔드 개발자 : (HTML/JS 소스를 보며) “이 버튼에 onclick 이벤트가 발생하면 POST로 넘겨줍니다”
-   개발자 : (백엔드 소스를 보며) “account table에 insert 합니다”

회원가입 페이지 하나를 두고도 각 담당자가 사용하는 어휘가 이렇게도 다양합니다. 기획자의 user와 개발자의 account 는 비슷한 의미를 가지지만 다른 어휘들로 표현될 수 있습니다. 기획자의 user는 개발자의 입장에서는 account 뿐 아니라 session 까지 포함하는 상위개념이라고도 할 수 있겠네요. 

Semantic URL은 일반적으로 “/user/signup”, “/board/(show)list” 과 같이 주체(Subject) 혹은 객체(Object)의 행동(Action)까지 한 덩이로 이루어 집니다. 여기서 주체와 객체는 프로젝트 내의 주요개념에 대응하고, 행동은 기능에 대응시킬 수 있을 것입니다. 

**개념**

**기능**

**URL**

사용자 

가입 

/user/join 

 

탈퇴 

/user/leave 

 

정보 수정 

/user/update 

게시판 

리스트 

/board/(게시판이름)/list 

 

글 보기 

/board/(게시판이름)/(글번호) 

 

글 수정 

/board/(게시판이름)/update/(글번호)  

기능정의에서부터 Semantic URL을 활용하면, 프로젝트에서 통용되는 개념과 기능을 일관성있고 쉽게 파악할 수 있지 않을까요? 

2\. 산출물들 간 디렉토리 구조 및 파일 이름을 통일 
-------------------------------

디자인 – 프론트엔드 – 백엔드에 이르기까지 프로젝트 구성원들은 서로다른 네이밍 노하우를 가지고 있습니다. 따라서 이에 대한 가이드가 없다면 “회원가입.psd”, “join.html”, “signup.jsp”, 심지어 “project\_0001.html” 에 이르기까지 구성원들 수 만큼이나 다양한 파일이름이 등장합니다. 문제가 생겨 파일들을 뒤져보려고 하면 “기획서의 이 페이지가 어떤 파일이에요?” 를 반복해야 합니다. 커뮤니케이션 로스가 생깁니다. 

생각해보세요. 문제가 생기면 해결하는데 시간을 온전히 쏟아도 모자랄텐데 그에 앞서 메신저로 파일명을 물어보거나, 메일에서 파일이름을 검색하고 있는 것은 한심하지 않나요? 

Semantic URL 을 디렉토리 – 파일 구조로 대입해 user/signup.psd, user/signup.jsp, user/signup.html 로 산출물을 구조화하면   서로의 산출물을 열람하기가 월등히 쉬워질 겁니다. 파일이름이 곧 용도이므로, 전달하면서 “이 파일은 이런 용도고 기획서에서 어떤 이름 이고 ..”  와 같은 군색한 메일을 적을 이유가 없습니다.  

결론
--

도메인네임은 [http://kthcorp.com/](http://kthcorp.com/) 처럼 회사를 대표하거나 http://im-in.com 처럼 프로젝트를 대표합니다. Semantic URL은 하위 URL을 구조화합니다. 하지만 많은 사람들이 도메인네임에는 수많은 고려를 하면서도 하위 URL에 대해서는 등한히 하는 경우가 많습니다. URL 을 결정하는 일은 막상 개발이 시작되고 난 후, 개발자의 몫이 될 때가 있습니다. 이런 경우는 곧 개발자의 작명센스(?)나 경험, 인터넷 한영사전에 의해 URL이 결정됩니다. 

하지만 개념과 기능을 정의하는 작업은 기획시기 부터 이루어지는 작업입니다. 따라서 Sementic URL에 대한 고민은 이 때부터 시작되어야 한다고 생각합니다. 서비스에 의미를 부여하는, 게다가 SEO 에까지 영향을 미치는 작업을 개발자의 잠깐 고민으로 결정해버릴 수는 없으니까요.  

ps : 저는 영어를 못하는데요! 라고 걱정하시는 분이 있다면, 페이스북에서 제공하는 [‘Translations’](http://www.facebook.com/?sk=translations) 이 참고할만 합니다. 예를 들어 [“탈퇴”라는 키워드를 검색하면](http://www.facebook.com/?q=%ED%83%88%ED%87%B4&sk=trans_search) 굉장히 다양한 표현들이 등장합니다. 적합한 표현을 찾아 프로젝트만의 사전을 만들어두는 것도 좋은 방법이 될 수 있습니다.