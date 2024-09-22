---
title: "브라우저 자동완성 기능의 미래"
author: elegantcoder
date: 2013-02-04T05:09:12
publishDate: 2013-02-04T05:09:12
summary: "Form element 에 autocomplete 기능이 소개된 것은 1999년 Internet Explorer 5이 최초였다. 이후 각 브라우저 별로 산발적으로 지원하던 자동완성 기능은 HTML5에 이르러 표준화 되었다. 
	HTML5 스펙은 2012년 12월 17일 버전으로 기준으로 함.
이전 스펙에서 autocomplete attriute 는 &#8220;on&#8221; 또는 &#8220;off&#8221; 값만 가질 수 있었으나 최근의 스펙에서는 on, off 외에 공백으로 분리된 토큰들을 가질 수 있다. 이에 따르면 attribute 이 on, off 가 아니라면 아래와 같은 순서로 동작한다.
1. 첫 8글자가 &#8220;section-&#8221; 으로 시작하는 경우 필드가 이름을 가진 그룹에 속함을 나타낼 수 있다.
"
url: "/posts/future-of-autocompletion"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:32
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
Form element 에 autocomplete 기능이 소개된 것은 1999년 Internet Explorer 5이 최초였다. 이후 각 브라우저 별로 산발적으로 지원하던 자동완성 기능은 HTML5에 이르러 표준화 되었다.

```
HTML5 스펙은 2012년 12월 17일 버전으로 기준으로 함.
```

이전 스펙에서 autocomplete attriute 는 “on” 또는 “off” 값만 가질 수 있었으나 최근의 스펙에서는 on, off 외에 공백으로 분리된 토큰들을 가질 수 있다. 이에 따르면 attribute 이 on, off 가 아니라면 아래와 같은 순서로 동작한다.

1.  첫 8글자가 “section-” 으로 시작하는 경우 필드가 이름을 가진 그룹에 속함을 나타낼 수 있다. (선택사항)

예를들어, 양식에 두개의 배송주소가 있을 때 아래처럼 마크업 할 수 있다.

```
<fieldset>
 <legend>Ship the blue gift to...</legend>
 <p> <label> Address:     <input name=ba autocomplete="section-blue shipping street-address"> </label>
 <p> <label> City:        <input name=bc autocomplete="section-blue shipping region"> </label>
 <p> <label> Postal Code: <input name=bp autocomplete="section-blue shipping postal-code"> </label>
</fieldset>
<fieldset>
 <legend>Ship the red gift to...</legend>
 <p> <label> Address:     <input name=ra autocomplete="section-red shipping street-address"> </label>
 <p> <label> City:        <input name=rc autocomplete="section-red shipping region"> </label>
 <p> <label> Postal Code: <input name=rp autocomplete="section-red shipping country"> </label>
</fieldset>
```

1.  어떤 토큰은 둘 중 하나일 수 있다. (선택사항)
    
    -   shipping 은 이 필드가 배송 주소 또는 연락처 정보의 일부임을 나타냄
    -   billing 은 이 필드가 청구 주소 또는 연락처 정보의 일부임을 나타냄
2.  다음 두 옵션 중 하나
    
    -   어떤 토큰이 아래 autofill field string 중 하나:
        -   “name”
        -   “honorific-prefix”
        -   “given-name”
        -   “additional-name”
        -   “family-name”
        -   “honorific-suffix”
        -   “nickname”
        -   “organization-title”
        -   “organization”
        -   “street-address”
        -   “address-line1”
        -   “address-line2”
        -   “address-line3”
        -   “locality”
        -   “region”
        -   “country”
        -   “postal-code”
        -   “cc-name”
        -   “cc-given-name”
        -   “cc-additional-name”
        -   “cc-family-name”
        -   “cc-number”
        -   “cc-exp”
        -   “cc-exp-month”
        -   “cc-exp-year”
        -   “cc-csc”
        -   “language”
        -   “bday”
        -   “bday-day”
        -   “bday-month”
        -   “bday-year”
        -   “sex”
        -   “url”
        -   “photo”

-   아래 순서대로
    1.  어떤 토큰이 아래 값들 중 하나 (선택사항)
        -   home, work, mobile, fax, pager
    2.  어떤 토큰이 아래 autofill field 스트링 중 하나
        -   tel, tel-country-code, tel-national, tel-area-code, tel-local, tel-local-prefix, tel-local-suffix, tel-extension, email, impp

즉, 이 스펙에 따르면 아래와 같은 경우의 수가 존재한다.

```
<input name="country" autocomplete="on">
<input name="country" autocomplete="off">
<input name="country" autocomplete="country">
<input name="country" autocomplete="shipping country">
<input name="country" autocomplete="section-a shipping country">
<input name="billing_home_tel" autocomplete="section-a billing home tel">
```

이런 상세한 분류를 통해 브라우저가 앞으로 더 강력한 자동완성기능을 갖게 될 것이다.

예를 들어 명세서 청구주소는 집이지만, 배송은 회사에서 받게 되는 경우가 있을 수 있다. 이런 경우 billing 에 집 주소, 집 연락처가 저장될 것이고 shipping 에는 회사 연락처 등이 저장되는 형태로 세분화가 가능해질 것이다.

참고
--

-   [New Features in Internet Explorer 5 – Microsoft](http://support.microsoft.com/kb/221787)
-   [4.10.19.8 Autofilling form controls: the autocomplete attribute – W3C HTML5 Specification](http://www.w3.org/TR/html51/forms.html#autofilling-form-controls:-the-autocomplete-attribute)