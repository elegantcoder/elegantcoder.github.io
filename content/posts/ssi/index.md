---
title: "SSI를 이용한 HTML 산출물 모듈화"
author: elegantcoder
date: 2012-08-31T12:13:34
publishDate: 2012-08-31T12:13:34
summary: "개요
SSI 는 Server Side Include 의 약자로, 웹서버에서 직접 제공하는 서버사이드 스크립트 언어 입니다.
PHP, ASP, JSP 등도 서버사이드 스크립트 언어의 범주에 들어가지만, SSI 는 대부분의 웹서버에서 직접 지원하고, 문법이 HTML 형식이라는 특징이 있습니다.
문법을 살펴보면,
&lt;!&#8211;#include virtual=&#8221;../quote.txt&#8221; &#8211;&gt;
와 같이 HTML 의 주석 문법에 #을 붙인 형태로 지시자를 작성하고, attirubte 을 사용해 파라미터를 전달합니다.
일반적인 지시자들
일반적으로 많이 사용하는 지시자들 입니다.
"
url: "/posts/ssi"
aliases: ["/ssi"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:44
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
==

SSI 는 Server Side Include 의 약자로, 웹서버에서 직접 제공하는 서버사이드 스크립트 언어 입니다.

PHP, ASP, JSP 등도 서버사이드 스크립트 언어의 범주에 들어가지만, SSI 는 대부분의 웹서버에서 직접 지원하고, 문법이 HTML 형식이라는 특징이 있습니다.

문법을 살펴보면,

```
<!--#include virtual="../quote.txt" -->
```

와 같이 HTML 의 주석 문법에 #을 붙인 형태로 지시자를 작성하고, attirubte 을 사용해 파라미터를 전달합니다.

일반적인 지시자들
---------

일반적으로 많이 사용하는 지시자들 입니다.

#include

다른 파일을 이 파일 내부로 불러들이기

#exec

외부 명령 실행

#set

변수 선언

#echo

변수 프린트

#if, #else, #elif, #endif

조건문

특징
==

대부분의 웹서버에서 동일 문법으로 사용 가능
------------------------

Apache, Nginx, Tomcat, Web sphere 등 다양한 웹 서버에서 동일한 문법으로 사용 가능합니다. 확인결과 Apache, Nginx, Tomcat 에서는 설치가 필요하지 않고, 설정만으로 곧바로 사용 가능합니다.

제한된 기능
------

SSI 는 간단한 스크립트를 작성하는데 최적화 된 언어입니다. 반복문이 없고, 모든 문법이 HTML 주석을 포함한 형태여서 코드가 길어집니다. 따라서 복잡한 작업에는 어울리지 않습니다.

활용방안
====

기능이 작고, 대부분의 웹서버와 호환된다는 면에서 HTML 산출물의 모듈별관리에 매우 적합합니다. 게다가 문법이 웹 퍼블리셔에게 친숙한 HTML 문법으로 작성하므로 러닝커브 면에서 꽤 괜찮은 선택입니다.

**KTH FI팀에서는 현재 SSI를 활용해 HTML을 모듈별로 관리하고 있습니다.**

HTML 의 모듈화는 서버사이드 언어의 지원을 받지 않으면 실현 불가능한 명제 입니다. 그래서 많은 퍼블리셔들이 반복되는 부분에 대해 아얘 모듈화하지 않거나, PHP 같은 언어의 지원을 받아 구현을 합니다.

아직 모듈화하고 있지 않다면 꼭 고려해보세요.
-------------------------

HTML 산출물을 작성할 때 Plain HTML 을 사용한다면 반복되는 코드는 모두 중복될 수 밖에 없습니다. 이 방법은 다른 파일에 의존성없이 완전한 결과물을 볼 수 있다는 장점이 있지만, 중복되는 부분에 변경이 발생하면 일괄 변경해야 하는 어려움이 있습니다.

모듈화된 HTML 을 사용하면

-   변경이 간편해집니다.
-   모듈을 퍼블리셔가 직접 지정하므로 개발자와의 커뮤니케이션 공수가 줄어듭니다.
-   파일 내에서 공통 부분이 차지하는 코드의 양이 적어지므로 실제로 작업할 내용에 대해 집중도가 높아집니다.

만약 다른 방법으로 목적을 달성하고 있더라도 한번 고려해보세요.
-----------------------------------

사실 SSI는 이미 많이 알려진 방법이고, 굉장히 오래 전에 사용되었던 방법입니다. 하지만 시선을 HTML 산출물의 모듈관리로 돌려보면 이보다 좋은 솔루션을 찾기 어렵습니다.

-   러닝커브가 크지 않습니다.
-   대부분의 웹서버에서 동작합니다.
-   웹서버에 언어모듈을 올리지 않아도 되므로 더 간단한 구조로 구현할 수 있습니다.
-   (물론 mod\_includes 모듈이 로드되어야 하긴 하지만, 나도모르는 사이에 로드되어 있는 경우가 많고, 모듈 크기도 더 작습니다)

기능이 제한된다는 것은 일견 나쁠 수도 있지만 다시 생각해보면 장점으로 생각할 수도 있습니다. 단 몇가지를 위해 기능이 과도하게 많은 언어를 사용하는 것은 닭잡는데 소잡는 칼을 쓰는 것과 다르지 않기 때문입니다.

사용해보기
=====

설정
--

설정은 아파치를 기준으로 합니다.

### .htaccess 설정

```
# +Includes 를 넣었습니다. 이것은 httpd.conf 에서 <Directory> 설정을 상속받아 Includes를 추가해주어야 하므로
# Options Includes 가 아니라  Options +Includes 입니다.
# .htaccess 를 사용하려면 <Directory> 에서 AllowOverride All 로 설정하세요.
AddOutputFilter INCLUDES .html
Options +Includes
```

Include 사용해보기
-------------

설정이 끝나면 간단히 include 문을 테스트 해봅니다.

미리보기: [http://code.elegantcoder.com/blog-examples/SSI/html5\_boilerplate.html](http://code.elegantcoder.com/blog-examples/SSI/html5_boilerplate.html)

### 본체- html5\_boilerplate.html

-   include 문을 주의해서 봐주세요.
    =====================
    
    ```
    <!DOCTYPE html>
    <html lang="en">
      <head>
        <!--#include virtual="_header.html" -->
        <title>html5_boilerplate</title>
      </head>
    
      <body>
        <div>
          <header>
            <h1>html5_boilerplate</h1>
          </header>
          <nav>
            <p>
              <a href="/">Home</a>
            </p>
            <p>
              <a href="/contact">Contact</a>
            </p>
          </nav>
    
          <div>
    
          </div>
    
          <footer>
            <!--#include virtual="_footer.html" -->
          </footer>
        </div>
      </body>
    </html>
    ```
    

### header – \_header.html

```
     <meta charset="utf-8" />

    <!-- Always force latest IE rendering engine (even in intranet) & Chrome Frame
    Remove this if you use the .htaccess -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />

    <meta name="description" content="" />
    <meta name="author" content="elco" />

    <meta name="viewport" content="width=device-width; initial-scale=1.0" />

    <!-- Replace favicon.ico & apple-touch-icon.png in the root of your domain and delete these references -->
    <link rel="shortcut icon" href="/favicon.ico" />
    <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
```

### footer – \_footer.html

```
<p>
&copy; Copyright by elco
</p>
```

그 외의 활용 예
=========

User-Agent Sniffing
-------------------

모바일 웹에서 User Agent Sniffing 이 필요한 경우 JS를 사용하거나 다른 서버사이드 언어에 의존할 수 밖에 없는데, SSI 는 이런 간단한 작업에 적합합니다. Server 변수들은 바로 SSI 에서 사용가능하고, Apache 의 mod\_rewrite 를 통하면 URL 에 따른 변수 지정도 가능합니다.

미리보기- [http://code.elegantcoder.com/blog-examples/SSI/user-agent.html](http://code.elegantcoder.com/blog-examples/SSI/user-agent.html)

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>User Agent Sniffing Example</title>
    </head>
    <body>
        <h1>User Agent Sniffing Example</h1>
        <div><span>User-agent String:</span><span><!--#echo var="HTTP_USER_AGENT" --></span></div>
        <!--#if expr="$HTTP_USER_AGENT = /Chrome/" -->
        <!--#set var="ua_result" value="Chrome" -->
        <!--#elif expr="$HTTP_USER_AGENT = /Safari/" -->
        <!--#set var="ua_result" value="Safari" -->
        <!--#elif expr="$HTTP_USER_AGENT = /Firefox/" -->
        <!--#set var="ua_result" value="Firefox" -->
        <!--#elif expr="$HTTP_USER_AGENT = /MSIE/" -->
        <!--#set var="ua_result" value="Internet Explorer" -->
        <!--#endif -->
        <div><span>so, u r using : </span><span><!--#echo var="ua_result" --></span></div>
    </body>
</html>
```

참고
==

-   아파치 SSI 메뉴얼 (mod\_includes): [http://httpd.apache.org/docs/current/mod/mod\_include.html](http://httpd.apache.org/docs/current/mod/mod_include.html)
-   NginX SSI 메뉴얼 (HttpSsiModule): [http://wiki.nginx.org/HttpSsiModule](http://wiki.nginx.org/HttpSsiModule)
-   IIS SSI 메뉴얼 (Server-Side Include 지시문 사용): [http://technet.microsoft.com/ko-kr/library/cc758826(v=ws.10).aspx](http://technet.microsoft.com/ko-kr/library/cc758826\(v=ws.10\).aspx)
