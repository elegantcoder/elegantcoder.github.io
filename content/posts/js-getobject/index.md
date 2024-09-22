---
title: "중첩된.오브젝트.프로퍼티.가져오기.성공적 = getobject "
author: elegantcoder
date: 2015-06-17T17:44:37
publishDate: 2015-06-17T17:44:37
summary: "TypeError: Cannot read property ‘..&#8217; of undefined 는 매우 흔히 발생하고 오랫동안 개발자를 괴롭혀온 에러다. 이 에러는 오브젝트의 프로퍼티에 접근하려 하는데 그 오브젝트가 undefined 인 경우 발생한다. 즉, 아래 같은 상황이다. &gt; var opt = {}; undefined &gt; opt.a.b.c.d TypeError: Cannot read property &#8216;b&#8217; of undefined &#8230; &gt; 특히 API 연동처럼 외부에서 생성된 오브젝트를 가져와 [&hellip;]
"
url: "/posts/js-getobject"
titleImage: "javascript.png"
draft: false
lastmod: 2015-08-06T07:21:17
isCJKLanguage: true
categories:
  - dev
tags:
  - Cannot read property
  - getobject
  - nested
  - properties
  - undefined
  - 중첩
  - 프로퍼티
  - outdated
params:
  images:
    - "javascript.png"
---
TypeError: Cannot read property ‘..’ of undefined 는 매우 흔히 발생하고 오랫동안 개발자를 괴롭혀온 에러다.

이 에러는 오브젝트의 프로퍼티에 접근하려 하는데 그 오브젝트가 undefined 인 경우 발생한다. 즉, 아래 같은 상황이다.

```
> var opt = {};
undefined
> opt.a.b.c.d
TypeError: Cannot read property 'b' of undefined
...
> 
```

특히 API 연동처럼 외부에서 생성된 오브젝트를 가져와 사용할 때 프로퍼티가 누락되면 자주 만나게 된다. 특히 스키마가 없는 NoSQL 데이터베이스를 사용하는 경우에 자주 만나는 오류이기도 하다.

그래서 여러가지 방법으로 돌파를 시도하는데, 변하지 않는건 단계가 깊어질 수록 조건문이 길어진다는 것이다. 커피스크립트 등에서는 `?.` 이라는 존재연산자(existential operator)를 지원하고, [JavaScript 완벽가이드](http://www.aladin.co.kr/shop/wproduct.aspx?isbn=8991268412)에도 비슷한 코드가 수록되어 있다.

이렇게 중첩된 프로퍼티들을 조건문을 사용하지 않고 가져올 수 있다면 꽤 편리할텐데 이런 기능을 가진 패키지를 찾았다. 바로 [Ben Alman](http://benalman.com/) 이 만든 [getobject](http://github.com/cowboy/node-getobject) 이다. Ben Alman 은 Gruntjs 의 최초작성자이기도 하다. 이 패키지는 Gruntjs 에도 포함되어 있다. (사실은 이 패키지를 Gruntjs 코드를 살펴보다 알게되었다.)

패키지는 NodeJS 에서 사용할 수 있는 [node-getobject](http://github.com/cowboy/node-getobject) 와 jquery 및 클라이언트쪽 자바스크립트에서 사용할 수 있는 [jquery-getobject](http://github.com/cowboy/jquery-getobject) 패키지 두가지 이다. jquery-getobject 라는 프로젝트 명을 가지고 있긴 하지만 jquery 를 사용하지 않는 경우도 이 패키지를 사용할 수 있다.

node-getobject
--------------

먼저 node 쪽의 node-getobject 를 살펴보자. 프로젝트 홈은 [http://github.com/cowboy/node-getobject](http://github.com/cowboy/node-getobject) 이다.

### 설치

```
npm install getobject
```

### 사용법

```
getobject.get(object, parts, create);
```

중첩된 프로퍼티에서 값을 편리하게 가져온다. 값이 없는 경우에는 undefined 를 리턴한다.

-   obj 는 대상이 되는 오브젝트이다.
-   parts 는 오브젝트 내에 접근하는 중첩된 키들을 문자열로 적는다. 예를들어 opts.a.b.c 를 가져오고 싶다면 ‘a.b.c’ 로 적는 식이다.
-   create 은 parts 에 접근하는과정에 undefined 를 만나는 경우 이들을 생성할지 설정하는 옵션이다.

* * *

```
 getobject.set(obj, parts, value);
```

중첩된 프로퍼티에 값을 적는다.

-   obj 는 대상이 되는 오브젝트이다.
-   parts 는 오브젝트 내에 접근하는 중첩된 키들을 문자열로 적는다. .get 에서의 parts 와 같다.
-   value 는 적을 값이다.

* * *

```
getobject.exists(obj, parts);
```

중첩된 프로퍼티에 값이 존재하는지 검사한다.

### 예제

```
var getobject = require(‘getobject’);
var opt = {};
getobject.get(opt, ‘a.b.c.d.e’, true);
typeof getoject.get(opt, ‘a.b.c.d’);  // object
getobject.set(opt, ‘a.b.c.d.e’, ‘hello');
getobject.get(opt, ‘a.b.c.d.e’); // hello
```

jquery-getobject
----------------

프로젝트 홈은 [http://github.com/cowboy/jquery-getobject](http://github.com/cowboy/jquery-getobject) 이다.

클라이언트측 자바스크립트에서의 사용법은 Node 에서의 사용과는 조금 다르다.

jQuery 를 사용중이라면 jQuery 내의 메서드처럼 동작하고 없는 경우에는 window.Cowboy 라는 객체 아래에 메서드들이 위치한다.

즉, window.jQuery.getObject(),window.jQuery.setObject() (물론, window.jQuery 는 $ 로 줄여쓰는 경우가 많다.) 이거나 window.Cowboy.getObject(), window.Cowboy.setObject() 가 된다.

### 사용법

```
$.getObject(parts, create, obj);
```

중첩된 프로퍼티에서 값을 편리하게 가져온다. 값이 없는 경우에는 undefined 를 리턴한다.

-   parts 는 오브젝트 내에 접근하는 중첩된 키들을 문자열로 적는다. 예를들어 opts.a.b.c 를 가져오고 싶다면 ‘a.b.c’ 로 적는 식이다.
-   create 은 parts 에 접근하는과정에 undefined 를 만나는 경우 이들을 생성할지 설정하는 옵션이다.
-   obj 는 대상이 되는 오브젝트이다.

* * *

```
$.setObject(name, value, context);
```

중첩된 프로퍼티에 값을 적는 메서드이다.

-   parts 는 오브젝트 내에 접근하는 중첩된 키들을 문자열로 적는다. .getObject 에서의 parts 와 같다.
-   value 는 적을 값이다.
-   context 는 대상이 되는 오브젝트이다. context 는 생략가능하고, 생략한 경우 window 객체에서 찾아내려간다.

* * *

```
$.exists(parts, obj)
```

중첩된 프로퍼티에 값이 존재하는지 검사한다.

### 예제

jQuery 를 사용 중인경우

```
window.$ === window.jQuery; //true
var opt = {};
$.getObject('a.b.c.d.e', true, opt); //undefined 
$.setObject('a.b.c.d.e', 'hello', opt);
```

jQuery 를 사용하지 않는경우

```
window.Cowboy.getObject('a.b.c.d.e', true, opt); //undefined        
window.Cowboy.setObject('a.b.c.d.e', 'hello', opt);
```