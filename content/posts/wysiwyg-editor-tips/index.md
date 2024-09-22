---
title: "위지윅(WYSIWYG) 에디터를 만들고자 할 때 고려할 사항"
author: elegantcoder
date: 2011-05-08T13:16:04
publishDate: 2011-05-08T13:16:04
summary: "원문은 http://bit.ly/ksonHb 에서 보실 수 있습니다.
1. iframe docs vs. contenteditable divs
위지윅에디터를 제작하고자 할 때 iframe을 사용할 지, 어느 엘리먼트에나 존재하는 contenteditable 속성을 이용할 지 고민될 수 있습니다. 답변자는 iframe을 추천합니다. 그 이유로:&nbsp;
iframe내에서는 문서 타입, CSS, script 등을 이용해 완전한 제어를 할 수 있습니다. 여러 다른 페이지내에서 통일된 액션과 모습을 보여주는 데에 꼭 필요합니다.
특히 Firefox에서 contenteditable 속성을 가진 엘리먼트가 매우 버그가 많은데, 이것은 몇 년 전부터 있어왔고 아주 잘 동작하는 document의 designMode속성에 비해(pre-1.0 부터인가? 확실하지 않습니다) 이 속성이 비교적 최근에(3.0버전부터) 도입되었기 때문입니다.
&nbsp;
2.
"
url: "/posts/wysiwyg-editor-tips"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:18:02
isCJKLanguage: true
categories:
  - 번역
tags:
  - 번역
  - outdated
params:
  images:
    - "undefined"
---
원문은 http://bit.ly/ksonHb 에서 보실 수 있습니다.

1\. iframe docs vs. contenteditable divs
----------------------------------------

위지윅에디터를 제작하고자 할 때 iframe을 사용할 지, 어느 엘리먼트에나 존재하는 contenteditable 속성을 이용할 지 고민될 수 있습니다. 답변자는 iframe을 추천합니다. 그 이유로:

> iframe내에서는 문서 타입, CSS, script 등을 이용해 완전한 제어를 할 수 있습니다. 여러 다른 페이지내에서 통일된 액션과 모습을 보여주는 데에 꼭 필요합니다. 특히 Firefox에서 contenteditable 속성을 가진 엘리먼트가 매우 버그가 많은데, 이것은 몇 년 전부터 있어왔고 아주 잘 동작하는 document의 designMode속성에 비해(pre-1.0 부터인가? 확실하지 않습니다) 이 속성이 비교적 최근에(3.0버전부터) 도입되었기 때문입니다.

2\. Cross browser styling
-------------------------

execCommand는 볼드, 이탤릭 등 엘리먼트에 스타일을 입히도록 하는 역할을 합니다. 문제는 각 브라우저 마다 이 메서드를 실행했을 때 어떤 브라우저는 볼드를 `<b>`로, 어떤 브라우저는 `<strong>`으로 표시하는 것과 같은 문제가 발생할 수 있습니다. 이것을 통일 시키는 방안에 대해 답변자는 이렇게 이야기 합니다.

> 만약 여러 다른 브라우저에서 통일된 결과물을 얻는 것이 중요하다면, 아마 자기만의 스타일링 코드를 작성해야 할 겁니다. 그렇지만, 이렇게 하면 브라우저에 내장된 되돌리기(undo) 작업을 불가능하게 할 것이므로 자기만의 되돌리기(undo)/다시실행하기(redo) 코드를 만들어야 할 겁니다.

3\. adding items to the undo chain
----------------------------------

> Q:브라우저에 내장된 되돌리기/다시실행하기 작업이 저장된 배열이 있을까요? 그리고 그것을 수정할 수 있을까요?
> 
> A: 브라우저 내장의 되돌리기 스택에 접근할 수 있는 방법은 없습니다. 스스로 작성하셔야 합니다.

4\. keeping the text selection
------------------------------

> Q: 글자체를 선택하는 것과 같이 포커스가 이동하면서 블럭지정이 풀려버리는데, 블럭지정한 텍스트를 어떻게 유지할 수 있을까요? Rangy나 Google closure 외에 다른 라이브러리가 있습니까?
> 
> A: 이 질문이 가장 흥미롭습니다. 바로 제가 Rangy를 작성했거든요. 이와 같은 일을 하는데에 더 좋은 라이브러리는 없다고 생각합니다. Google Closure는 범위/선택과 관련한 API를 가지고 있지 않지만 DOM Range와 일반적인 브라우저의 Selection 객체보다 더 우선한 자신만의 인터페이스를 사용한다고 생각합니다. IERange는 Rangy와 비슷한 아이디어를 가진 라이브러리이지만 구현된 범위가 적으므로 지금당장은 고려대상이 아니라고 생각합니다.

p.s. 재미있는 것은, 그 아래의 답변입니다. “진심으로 말하는데 그냥 나와있는 것을 쓰세요” 라는데, 무려 3명이나 추천했군요 🙂