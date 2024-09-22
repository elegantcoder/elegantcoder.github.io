---
title: "슬쩍 떠보는 npm 과 package.json"
author: elegantcoder
date: 2015-07-21T08:13:44
publishDate: 2015-07-21T08:13:44
summary: "GruntJS 와 같이 Node.js 로 만들어진 유틸리티들은 npm 을 통해 설치하고 사용하는 것이 쉽고 편리하다. 하지만 Node.js 를 깊이 공부해보려는 목적이 아니라면 굳이 npm 에 대해 깊이 알 필요는 없다. 그런 사람들과 입문자들을 위해 npm 을 통한 패키지의 설치/제거방법과 package.json 에 대해 간단히 알아보려고 한다. npm 는 Node Package Manager 의 약자이다. 보통 npm 은 [&hellip;]
"
url: "/posts/beginning-npm-package"
titleImage: "npm-logo.png"
draft: false
lastmod: 2015-07-27T03:12:24
isCJKLanguage: true
categories:
  - dev
tags:

  - outdated
params:
  images:
    - "npm-logo.png"
---
GruntJS 와 같이 Node.js 로 만들어진 유틸리티들은 npm 을 통해 설치하고 사용하는 것이 쉽고 편리하다. 하지만 Node.js 를 깊이 공부해보려는 목적이 아니라면 굳이 npm 에 대해 깊이 알 필요는 없다. 그런 사람들과 입문자들을 위해 npm 을 통한 패키지의 설치/제거방법과 package.json 에 대해 간단히 알아보려고 한다.

npm 는 Node Package Manager 의 약자이다. 보통 npm 은 두가지 의미로 사용된다. 첫번째는 오픈소스로 작성된 Node.js 모듈들이 등록된 저장소인 [http://npmjs.com](http://npmjs.com) 를 의미한다. 두번째는 패키지를 인스톨하고 의존성을 관리하는 자바스크립트로 작성된 커맨드라인 유틸리티이다. 커맨드라인 유틸리티는 npmjs.com 의 저장소로 부터 패키지를 찾고 설치하는 등의 기능을 사용할 수 있다. 이 글에서 npm 은 주로 커맨드라인 유틸리티의 의미로 사용하고, 첫번째 의미의 npm 은 npm 저장소라고 부를 것이다.

패키지 관리가 필요한 이유
--------------

일반적으로 Node.js 의 프로젝트는 의존하는 다른 패키지의 소스코드를 포함하지 않는다. 대신 의존하는 패키지들을 `package.json` 에 명시하고 개발자들은 npm 을 사용해 각자 설치해 사용한다. 특히 패키지들 중에는 C 코드를 컴파일해야 하는 것들도 있다. 이 때 컴파일된 결과물은 OS 나 시스템에 의존적이기 때문에 패키지에 포함되지 않는다. 따라서 다른 패키지의 소스코드를 포함하지 않는 것이 원칙이고 패키지 설치와 제거를 위해 npm 기본사용법을 알아야 한다.

패키지 설치와 제거
----------

### 패키지 설치

```
npm install <패키지이름|패키지외부URL>
```

패키지 설치를 위해서는 install 명령을 사용한다. npm 저장소에 위치한 패키지들은 패키지이름을 적으면 설치된다. npm 패키지 외에 git 저장소나 외부 URL 에 위치한 패키지들도 설치도 가능하다. 외부 패키지 설치와 관련한 내용은 [공식문서의 Install Package](https://docs.npmjs.com/cli/install) 와 [공식문서의 Specifics of npm’s package.json handling](https://docs.npmjs.com/files/package.json#dependencies)에 정리되어 있다.

#### 지역패키지, 전역패키지 지정 옵션

npm 패키지는 설치되는 위치에 따라 지역패키지와 전역패키지로 나눌 수 있다.

전역(global)패키지는 시스템 전체에서 사용할 수 있도록 설치되는 패키지이다. 특히 명령어를 사용해야하는 경우에 많이 사용한다. 예를들어 `grunt-cli` 패키지는 `grunt` 명령을 포함하는데, 이 명령은 시스템 어디에서든 실행될 수 있어야 하므로 전역 패키지로 분류할 수 있다. 전역패키지로 설치하기 위해서는 전역을 의미하는 `--global`(또는 `-g`) 옵션을 사용한다.

```
npm install <패키지이름|패키지외부URL> --global
```

지역(local)패키지는 현재 프로젝트에만 한정해 사용하는 패키지를 말한다. 이 패키지들은 프로젝트 루트 디렉토리(`package.json` 이 있는 디렉토리)의 바로 아래에 `node_modules` 디렉토리에 설치된다. `package.json` 을 찾을 수 없으면 현재 디렉토리에 바로 아래에 `node_modules` 디렉토리를 만들고 여기에 패키지를 설치한다. `--global` 옵션을 명시하지 않은 경우 지역패키지로 설치된다.

#### 의존성 명시 옵션

package.json 에는 프로젝트가 의존하고 있는 다른 프로젝트를 명시할 수 있다. 특히 유닛테스트용 패키지나 `grunt` 태스크 플러그인처럼 프로젝트를 개발하거나 테스트할 때만 필요한 패키지들만 따로 명시해 설치할 수도 있다.

`--save` : 패키지를 설치하고 `package.json` 의 `dependencies` 항목에 설치한 패키지의 이름과 버전을 명시한다.

`--save-dev` : 패키지를 설치하고 `package.json` 의 `devDependencies` 항목에 설치한 패키지의 이름과 버전을 명시한다.

```
npm install <패키지이름|패키지외부URL> [--save-dev|--save]
```

#### 의존성 전체 설치

```
npm install [--production]
```

`package.json` 에 `dependencies` 와 `devDependencies` 에 명시된 의존성 패키지들은 다른 옵션없이 `npm install` 명령으로 설치할 수 있다.

프로젝트를 개발/테스트하려는 것이 아니라 활용만 하려는 목적이라면 개발의존성을 설치하는 것이 불필요하므로 `devDependencies` 의 패키지를 제외하고 설치할 수도 있다. 이 때는 `--production` 옵션을 사용한다.

### 패키지 제거

```
npm uninstall <패키지이름> [--save-dev|--save|--global]
```

패키지를 제거하는 명령은 `npm uninstall` 이다. `npm install` 과 마찬가지로 `--global` `--save-dev` `--save` 옵션을 사용할 수 있다.

주의할 점은 `--save` 나 `--save-dev` 옵션을 사용해 의존성을 명시한 패키지들은 제거할 때도 `--save` 나 `--save-dev` 를 사용해야 `package.json` 의 의존성 항목을 제거한다.

마찬가지로 전역패키지에서 찾아 제거하려면 `--global` 를 명시해야한다.

### 짧은 이름 사용하기

npm 은 명령어 중엔 별명을 사용할 수 있는 명령들이 있다. 예를 들어 `npm install` 은 `npm i` 로 줄여 쓸 수 있다. 그리고 `npm uninstall` 은 `npm u` 나 `npm rm`, `npm r` 으로 줄여 사용할 수 있다.

### package.json 생성

```
npm init [--yes]
```

package.json 은 정해진 규칙이 있는 JSON 형식의 파일이다. 하지만 세부 내용들을 모두 외워 생성하는 것은 어렵다. 이 때 `npm init` 명령을 통해 `package.json` 을 쉽게 생성할 수 있다.

`npm init` 은 대화형 인터페이스로 package.json 을 생성하도록 도와준다. 이 때 모든 값을 기본값으로 채우고 싶다면 `-yes`(또는 `-y`) 옵션을 사용할 수 있다. pacakge.json 의 일반적인 항목을 자동으로 채워 생성한다.

package.json 의 주요항목
-------------------

package.json 은 npm 이 사용하는 설정 파일이다. 패키지의 이름 및 의존성 등이 이 파일에 명시된다. 이 파일의 주요항목을 살펴보자.

`name` 프로젝트의 이름이다. 기본값은 프로젝트의 디렉토리 이름이다.

`version` 프로젝트의 버전이다. Node.js 패키지의 버전은 유의적 버전(Semver: Semantic Versioning) 의 체계를 사용한다. 이 체계는 버전번호를 주버전(主, Major), 부버전(部, Minor), 수버전(修, Patch) 로 구분해 3개의 점(dot) 으로 표현한다. 즉, 2.1.3 버전이 있다고 한다면, 주버전은 2, 부버전은 1, 수버전은 3이다. `name`과 `version` 항목은 package.json 에서 가장 중요한 정보이다. 이 두가지 정보를 결합해 패키지의 식별자로 사용한다. 따라서 패키지의 내용이 변경되면 꼭 `version` 항목을 변경해야한다. 참고로 `npm version [major|minor|patch]` 명령을 사용하면 자동으로 package.json 의 `version` 항목을 변경하고 Git 저장소에 버전이름(v2.1.3 형식)으로 태그를 달 수 있다.

(참고: 유의적 버전에 대해서는 [http://semver.org/lang/ko/](http://semver.org/lang/ko/) 를 참고하자.)

`description` 프로젝트의 구체적인 설명이다.

`main` 이 패키지의 엔트리포인트가 되는 자바스크립트 파일경로를 명시한다. 만약 패키지의 이름이 `foo` 라면 다른 패키지에서는 `require('foo')` 문으로 이 패키지를 로드한다. 이 때 `main` 에 지정된 파일을 로드하게 된다.

`scripts` 패키지에서 반복적으로 사용할 주요명령들을 지정해 사용할 수 있다. 기본값으로 생성된 `test` 를 실행하기 위해서는 `npm test` 으로 실행할 수 있다.

`keywords` 이 패키지를 설명하는 키워드들이다. 이렇게 지정된 키워드들은 `npm search` 명령으로 패키지를 검색할 때 검색어로 사용된다.

`license` 패키지의 라이선스 정책을 명시한다. 기본값은 `ISC` 이다. 참고로 `ISC` 는 INTERNET SYSTEMS CONSORTIUM 의 약자로 이 단체에서 제정한 공개 소프트웨어 라이선스이다.

`author`, `contributors` 객체형식 또는 문자열 형식으로 개발자의 이름과 이메일을 명시한다. `author` 는 원 저작자이기 때문에 문자열형식이나 객체형식으로 한명만 지정하고, `contributors` 는 여러 사람이 될 수 있으므로 배열 형식으로 지정한다. 사람을 표현할 때는 아래와 같이 이름, 이메일, URL을 명시할 수 있고, 이메일과 URL은 선택사항이다.

객체 형식으로 지정한 예

```
{ "name" : "Constantine Kim"
, "email" : "elegantcoder+nospam@gmail.com"
, "url" : "http://elegantcoder.com/"
}
```

문자열로 표현한 예

```
"Constantine Kim <elegeantcoder+nospam@gmail.com> (http://elegantcoder.com/)"
```

`dependencies`, `devDependencies` 이 패키지가 다른 패키지에 의존할 경우 의존성에 대한 항목이다. `foo` 라는 프로젝트의 2.x 버전에 의존적이라면 아래와 같이 명시한다.

```
{
  "dependencies" :
    "foo" : "2.x"
  }
}
```

이렇게 이름과 버전을 명시한 패키지들은 npm 저장소로부터 설치된다. 앞서 패키지 설치 장에서 봤듯 npm 저장소 외에도 git 저장소나 외부 URL 의 패키지도 명시할 수 있다.

`dependencies` 는 이 패키지에 의존하는 다른 프로젝트에서 구동시키기 위한 의존성이다. 즉, 이 패키지를 활용할 때 필요한 의존성을 명시한다. `npm install --save` 명령을 통해 패키지를 설치하면 이 항목에 프로젝트 정보가 저장된다.

`devDependencies` 에는 이 패키지를 테스트하거나 개발할 때 필요한 패키지들을 명시한다. `npm install --save-dev` 명령을 통해 패키지를 설치하면 이 항목에 프로젝트 정보가 저장된다.