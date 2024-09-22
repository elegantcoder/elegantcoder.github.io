---
title: "웹호스팅환경에서 git push 때 자동으로 pull 하기"
author: elegantcoder
date: 2013-03-13T14:26:51
publishDate: 2013-03-13T14:26:51
summary: "__ssh, bare repository, post-receive hook 을 사용해 로컬저장소에서 push 하면  호스팅 서버에서 자동으로 pull 받을 수 있는 환경을 꾸며봅니다.__
[Bitbucket](http://bitbucket.org) 에서 private 저장소를 만들어 버전관리를 하고 있는 프로젝트가 있다. 웹 프로젝트이고 이 프로젝트의 결과물은 Cafe24 에 월 500원짜리 웹호스팅에서 서비스된다.
처음엔 remote origin 을 Bitbucket 으로 두고, 이 저장소를 통해 로컬과 호스팅 서버간의 통신을 했다. 소스를 고치고 push 하고, 호스팅서버의 커맨드라인에서 pull 을 해야했으니 매우 불편했다.
"
url: "/posts/git-hook-in-hosting"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:13:38
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
**ssh, bare repository, post-receive hook 을 사용해 로컬저장소에서 push 하면 호스팅 서버에서 자동으로 pull 받을 수 있는 환경을 꾸며봅니다.**

[Bitbucket](http://bitbucket.org) 에서 private 저장소를 만들어 버전관리를 하고 있는 프로젝트가 있다. 웹 프로젝트이고 이 프로젝트의 결과물은 Cafe24 에 월 500원짜리 웹호스팅에서 서비스된다.

처음엔 remote origin 을 Bitbucket 으로 두고, 이 저장소를 통해 로컬과 호스팅 서버간의 통신을 했다. 소스를 고치고 push 하고, 호스팅서버의 커맨드라인에서 pull 을 해야했으니 매우 불편했다. ftp 때는 저장만 하면 자동으로 ftp 로 올려주는 프로그램도 있었는데, 더 좋은 도구를 쓰는데 해야할 일은 더 많아졌다.

물론 이럴 때 쓰기 위해 hook 이 존재하는 것이다. 호스팅서버에서 커맨드라인으로 git을 지원하므로 호스팅 서버에 베어 저장소를 만들어 hook 을 걸었다.

아래는 그 과정이다.

호스팅서버에서 git bare 저장소를 만든다.

```
elco@hosting $ git init --bare ~/myproject.git
```

git bare 저장소의 .git/hooks/post-receive.sample 파일을 편집해 post-receive 로 다시 저장한다. 나의 경우는 아래 처럼 했다. 아래 내용은 [getting “fatal: not a git repository: ‘.’” when using post-update hook to execute ‘git pull’ on another repo](http://stackoverflow.com/questions/4043609/getting-fatal-not-a-git-repository-when-using-post-update-hook-to-execut) 에서 참고했다.

```
cd ~/myproject || exit
unset GIT_DIR
git pull origin master
```

호스팅 서버에 서비스 디렉토리를 만들고 bare 저장소를 clone 해둔다.

```
elco@hosting $ git clone ~/myproject.git ./myproject
```

이미 존재하고 있는 로컬 저장소에서 새로운 git bare 저장소를 origin 으로 설정하고, push 한다. 기존의 Bitbucket 프로젝트는 bb 라는 이름으로 다시 저장했다. (이미 origin 이 존재한 경우만 아래와 같이 한다. origin 이 없었던 경우에는 clone 으로 한다.)

```
elco@mymac $ git remote rename origin bb
elco@mymac $ git remote add origin elco@hosting:~/myproject.git
elco@mymac $ git push origin master
```

로컬저장소에서 내용을 변경해 push 해 테스트한다.

```
elco@mymac $ git commit -am "hook test"
elco@mymac $ git push origin master
```

처음부터 hook 을 사용할 것을 생각하지 않아 remote origin을 지우고 다시 넣고 하긴 했지만, 작업을 진행하면서 계속 머리속을 맴도는 질문이 있었다.’이런 작업을 svn 에서는 어떻게 처리했지?’

서버를 떼어다 옮기거나 리모트 스크립트를 호출하지 않으면 중앙집중식에서는 이와 같은 방법으로는 처리할 수가 없다. 이것이 바로 중앙집중저장소와 분산저장소의 차이가 된다. 여러 서버 저장소를 두고 선택적으로 push 할 수 있다는 점. 이것이 분산 버전관리의 장점이라고 생각한다.

p.s

-   cafe24 의 git 을 활용하면 월 500원짜리 private git 저장소를 만들어 사용하는 것도 가능하다.
-   호스팅 서버에 Bitbucket 저장소도 remote 로 추가하고 hook 에서 git push bitbucket 이라는 라인도 추가해놓으면 push 한번에 bitbucket 과 호스팅 서버에 동시 저장도 가능하겠다.