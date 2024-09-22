---
title: "자바스크립트 파일이 세미콜론으로 시작하는 이유"
author: elegantcoder
date: 2013-03-03T14:15:09
publishDate: 2013-03-03T14:15:09
summary: "어떤 코드를 살펴보면 자바스크립트 파일이 세미콜론으로 시작하는 때가 있다.
	;(function () {})();
왜일까 궁금했었는데 스택 오버플로를 살펴보니 대략 아래와 같은 답변이다.
이 세미콜론은 자바스크립트 파일을 합칠 때(concatenating) 안정성에 도움이 된다.
"
url: "/posts/js-starts-with-semicolon"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:20
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
어떤 코드를 살펴보면 자바스크립트 파일이 세미콜론으로 시작하는 때가 있다.

```
;(function () {})();
```

왜일까 궁금했었는데 스택 오버플로를 살펴보니 대략 아래와 같은 답변이다.

이 세미콜론은 자바스크립트 파일을 합칠 때(concatenating) 안정성에 도움이 된다. HTTP Request 개수를 줄이기 위한 목적으로 파일을 합쳐서 전송할 파일의 개수를 줄이는 경우가 있는데, 이때 순서상 해당 파일의 직전 파일이 세미콜론으로 끝나지 않으면 파일을 합쳤을 때 제대로 동작하지 않기 때문이다.

다시말해, a.js 와 b.js 를 합쳐 ab.js를 만들 때

a.js

```
(function () {alert('a');})()
```

b.js

```
(function () {alert('b');})();
```

ab.js

```
(function () {alert('a');})()(function () {alert('b');})();
```

이렇게 되면 문제가 될 수 있다는 것이다.

closure compiler 나 uglify 같은 프로그램을 통해 작업을 수행한다면 문제가 없겠지만, 수동으로 합치는 경우는 문제가 발생할 수도 있겠다.

물론 이런 문제를 인식하고 있는 개발자라면 자기가 개발한 파일의 끝에 세미콜론은 꼭 붙여주겠지.

Semicolon on the beginning of the JavaScript Files  

=====================================================