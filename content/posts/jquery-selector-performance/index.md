---
title: "jQuery 요소 선택의 성능문제, 셀렉터와 탐색 중 어떤 것이 더 빠를까?"
author: elegantcoder
date: 2013-03-08T08:02:39
publishDate: 2013-03-08T08:02:39
summary: "스택오버플로를 둘러보다 아래와 같은 질문을 찾았다.
[Is jQuery traversal preferred over selectors?](http://stackoverflow.com/questions/15216838/is-jquery-traversal-preferred-over-selectors)
jQuery 에서 요소 선택의 성능문제를 묻는 질문인데, 
    $(&#8220;#vacations&#8221;).find(&#8220;li&#8221;).last();
    $(&#8220;#vacations li:last&#8221;);
이 두 가지중 어떤 것이 더 빠를까?
"
url: "/posts/jquery-selector-performance"
aliases: ["/jquery-selector-performance"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:13:42
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
스택오버플로를 둘러보다 아래와 같은 질문을 찾았다.

[Is jQuery traversal preferred over selectors?](http://stackoverflow.com/questions/15216838/is-jquery-traversal-preferred-over-selectors)

jQuery 에서 요소 선택의 성능문제를 묻는 질문인데,

```
$("#vacations").find("li").last();

$("#vacations li:last");
```

이 두 가지중 어떤 것이 더 빠를까? 하는 질문이다.

질문자도 그렇고, 개인적인 생각으로도 처음엔 후자가 더 빠르지 않을까했는데, 베스트 답변은 생각과는 조금 달랐다.

결론부터 말하자면 **jQuery 셀렉터가 아니라 표준 CSS 셀렉터를 사용하면 빠르다. 그렇지 않으면 탐색이 더 낫다**

답변이 좀 길지만, 읽어볼 만한 가치가 있어보여 아래 번역한다.

> There’s no cut and dry answer to this, but with respect to the :last selector you’re using, it’s a proprietary extension to the Selectors API standard.Because of this, it isn’t valid to use with the native .querySelectorAll method. What Sizzle does is basically try to use your selector with .querySelectorAll, and if it throws an Exception due to an invalid selector, it’ll default to a purely JavaScript based DOM selection/filtering. This means including selectors like :last will cause you to not get the speed boost of DOM selection with native code.

이 문제에 대한 명백한 해답은 없지만 질문에 나와있는 :last 셀렉터에 집중해 살펴보도록 하겠다. :last 셀렉터는 Selector API 표준을 jQuery 전용으로 확장한 것이다. 따라서 이 셀렉터는 네이티브 .querySelectorAll 메서드로는 사용할 수 없다.

Sizzle 은 기본적으로 요청받은 셀렉터를 .querySelectorAll 로 동작시켜본 후, 올바르지 않은 셀렉터로 인한 예외가 발생하면 순수 JavaScript 기반으로 DOM 선택, 필터링 한다.

즉 :last 등이 포함되면 네이티브 DOM 셀렉션의 빠른 속도를 얻을 수 없게 된다.

> Furthermore, there are optimizations included so that when your selector is very simple, like just an ID or an element name, the native getElementById and getElementsByTagName will be used, which are extremely fast; usually even faster than querySelectorAll. And since the .last() method just grabs the last item in the collection instead of filtering all the items, which is what Sizzle filters normally do (at least they used to), that also will give a boost.

게다가 셀렉터가 ID나 태그이름만 사용된 경우처럼 아주 간단하다면 정말 빠른 네이티브 getElementById 와 getElementsByTagName 을 사용하도록 최적화 로직이 내장되어있다. . 이들은 일반적으로 querySelectorAll 보다 훨씬 빠르다.

그리고 .last() 메서드는 일반적인 Sizzle 의 필터처럼 모든 아이템 중에서 필터링하는 것이 아니라 그저 컬렉션에서 마지막 아이템을 가져오는 형태로 동작하므로 여기서도 이점을 찾을 수 있다.

> IMO, keep away from the proprietary stuff. Now that .querySelectorAll is pretty much ubiquitous, there are real advantages to only using standards-compliant selectors. Do any further filtering post DOM selection. In the case of $(“#vacations”).find(“li”), don’t worry about the interim results. This will use getElementById followed by getElementsByTagName, and will be extremely fast. If you’re really super concerned about speed, reduce your usage of jQuery, and use the DOM directly.

개인적으로는 jQuery 전용 셀렉터를 배제하는 것이 좋겠다고 생각한다. .querySelectorAll 의 쓰임새가 훨씬 많은데, 표준 셀렉터를 사용해야만 그 이점을 누릴 수 있다. 그 이후의 필터링들은 DOM 셀렉팅 이후에 진행하도록 하자.

$(“#vacations”).find(“li”) 의 경우는 성능에 대해 크게 고민하지 않아도 된다. 이들은 getElementById 후에 getElementsByTagName 을 사용할 것이고, 이들은 정말 빠르기 때문이다.

진정으로 속도에 대해 고민한다면 jQuery 를 사용하지 말고 DOM 에 직접 접근하는 것이 좋다.

> You’ll currently find notes in the docs for selectors like :last, that warn you about the performance loss:
> 
> Because :last is a jQuery extension and not part of the CSS specification, queries using :last cannot take advantage of the performance boost provided by the native DOM querySelectorAll() method. To achieve the best performance when using :last to select elements, first select the elements using a pure CSS selector, then use .filter(“:last”).
> 
> But I’d disagree that .filter(“:last”) would be a good substitute. Much better would be methods like .last() that will target the element directly instead of filtering the set. I have a feeling that they just want people to keep using their non-standards-compliant selectors. IMO, you’re better of just forgetting about them.

jQuery 문서에도 :last 와 같은 셀렉터들의 성능문제에 대한 내용이 추가되었다.

:last 는 CSS 명세에 포함되지 않은 jQuery 의 확장이므로, :last 쿼리를 사용하는 것은 네이티브 DOM querySelectorAll() 의 성능향상을 기대할 수 없다. :last 를 사용해 요소를 선택할 때 최고의 성능을 얻으려면 먼저 순수 CSS 셀렉터로 요소를 선택하고, .filter(“:last”) 를 사용하라.

하지만 .filter(“:last”) 가 좋은 대체물이라고 생각하지는 않는다. .last() 와 같은 메서드들이 컬렉션을 필터링하기보다는 곧바로 요소를 잡아낼 수 있어서 훨씬 낫다. 내 느낌엔 이들이 사람들이 계속 비표준 셀렉터를 사용하길 원하고 있는 것 같다. 그러니 이건 그냥 잊는게 좋다고 생각한다.

그러니 jQuery 셀렉터보다는 표준 CSS 셀렉터를 숙지해 사용하도록 하자 🙂
