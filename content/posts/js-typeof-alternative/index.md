---
title: "자바스크립트의 변수형을 알아내는 다른 방법"
author: elegantcoder
date: 2013-03-23T14:10:55
publishDate: 2013-03-23T14:10:55
summary: "	JavaScript 의 변수형을 알아내는 데는 일반적으로 typeof 를 사용한다. 하지만 toString 을 사용하면 어떤 객체의 인스턴스인지까지 한번에 알아낼 수 있다.
## JavaScript 의 변수형을 알아내는 데는 일반적으로 typeof 를 사용한다.
이 방법이 가장 간단하고 일반적인 방법이다.
typeof 가 나타낼 수 있는 변수형은 아래와 같다.
undefined, object, boolean, number, string, function, object
typeof 가 변수에 대한 기본적인 호기심을 해결해주긴 하지만 몇가지 모자란 점이 있다.
"
url: "/posts/js-typeof-alternative"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:13:35
isCJKLanguage: true
categories:
  - dev
  - front-end
tags:
  - Front-end
  - outdated
params:
  images:
    - "undefined"
---
JavaScript 의 변수형을 알아내는 데는 일반적으로 typeof 를 사용한다. 하지만 toString 을 사용하면 어떤 객체의 인스턴스인지까지 한번에 알아낼 수 있다.

JavaScript 의 변수형을 알아내는 데는 일반적으로 typeof 를 사용한다.
----------------------------------------------

이 방법이 가장 간단하고 일반적인 방법이다.

typeof 가 나타낼 수 있는 변수형은 아래와 같다. undefined, object, boolean, number, string, function, object

typeof 가 변수에 대한 기본적인 호기심을 해결해주긴 하지만 몇가지 모자란 점이 있다. 가장 큰 문제는 어떤 객체의 인스턴스인지 정확히 알려주지 않는다는 점이다.

typeof null 은 ‘object’ 이다.

```
typeof null === 'object'; // true
```

typeof \[\] 도 ‘object’ 이다.

```
typeof [] === 'object'; //true
```

따라서 어떤 객체인지 더 알아보려면 조건판단을 더 해주어야 한다. 예를들면 날짜 객체인지 알아보려면 아래처럼 구현하기도 한다.

```
var date = new Date();
if (typeof date === 'object' && date instanceof Date) {
    console.log 'date';
}
```

[instanceof 는 같은 윈도우 환경에서만 작동하므로 위 방법은 프레임/아이프레임/팝업 같은 환경에서는 실패한다.](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/instanceof#instanceof_and_multiple_context_\(e.g._frames_or_windows\))

다른 방법
-----

변수형을 알아내는 다른 방법도 있다. 바로 .toString() 을 사용하는 것이다.

```
Object.prototype.toString.call({}) === "[object Object]" //true
```

이 방법은 객체 타입까지 구체적으로 표현해주는 장점이 있다.

```
Object.prototype.toString.call(new Date) === "[object Date]"; //true
Object.prototype.toString.call(/s/) === "[object RegExp]"; //true
Object.prototype.toString.call(/null/) === "[object Null]"; //true
Object.prototype.toString.call(1) === "[object Number]"; //true
```

어떤 원리인지 자세히 알아보고 싶다면 참고링크의 Fixing the JavaScript typeof operator 항목을 참조하기 바란다.

underscore.js 에서는 아래와 같이 사용하고 있다.

```
  // Add some isType methods: isArguments, isFunction, isString, isNumber, isDate, isRegExp.
  each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp'], function(name) {
    _['is' + name] = function(obj) {
      return toString.call(obj) == '[object ' + name + ']';
    };
  });
```

성능문제
----

하지만 이 방법의 단점은 성능이다. 사실 프로토타입의 객체를 call로 부르는 구현법이 typeof 라는 연산자보다 성능이 좋을리가 만무하다. 참고링크에 JSPerf 항목에서 underscore의 이전 구현/현재 구현간의 성능차이를 확인할 수 있다.

성능문제에도 불구하고 객체의 타입까지 손쉽게 알아낼 수 있으므로 효율적인 개발을 가능하게 할 것이고, 이 방식의 매우 큰 장점이다.

참고링크
----

-   [Fixing the JavaScript typeof operator](http://javascriptweblog.wordpress.com/2011/08/08/fixing-the-javascript-typeof-operator/)
-   [StackOverflow – Why does UnderscoreJS use toString.call() instead of typeof?](http://stackoverflow.com/questions/10394929/why-does-underscorejs-use-tostring-call-instead-of-typeof)
-   [JSPerf – Underscore.js isType Alternatives](http://jsperf.com/underscore-js-istype-alternatives)
-   [MDN – instanceof](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/instanceof#instanceof_and_multiple_context_\(e.g._frames_or_windows\))
-   [invalid.kr – javascript typeof check](http://invalid.kr/javascript-typeof-check/)