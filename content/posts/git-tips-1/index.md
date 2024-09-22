---
title: "Git pull 시에 Merge branch .. 메시지 나오지 않도록 하기"
author: elegantcoder
date: 2012-09-08T08:49:32
publishDate: 2012-09-08T08:49:32
summary: "## Git Pull 시에 머지 커밋하지 않기
    $> git pull
을 실행하면 리모트의 내용과 내 작업내역을 머지하게 되는데 이 때 머지되었다는 커밋을 한번 더 수행해주어야 한다.
이 경우 Merge branch &#8216;local branch name&#8217; of remote into &#8216;remote branch&#8217; 와 같은 자동 생성 커밋 메시지가 올라온다.
머지 커밋이 불필요하다고 생각하는 경우라면
    $> git pull &#8211;rebase
를 사용하면 머지 커밋이 필요없다.
***Github Client 의 Sync 기능이 내부적으로 이와 같이 동작 중임.
"
url: "/posts/git-tips-1"
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:14:41
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
Git Pull 시에 머지 커밋하지 않기
----------------------

```
$> git pull
```

을 실행하면 리모트의 내용과 내 작업내역을 머지하게 되는데 이 때 머지되었다는 커밋을 한번 더 수행해주어야 한다. 이 경우 Merge branch ‘local branch name’ of remote into ‘remote branch’ 와 같은 자동 생성 커밋 메시지가 올라온다.

머지 커밋이 불필요하다고 생각하는 경우라면

```
$> git pull --rebase
```

를 사용하면 머지 커밋이 필요없다.

***Github Client 의 Sync 기능이 내부적으로 이와 같이 동작 중임. [Github for Mac – help](http://mac.github.com/help.html)***

커밋하지 않고 Git Pull 받아오기
---------------------

SVN과 다르게 Git은 작업물이 완전히 커밋되어 있지 않으면 Pull 을 받아올 수 없다. 현재 작업상태를 유지하면서 Pull 을 받아오고 싶다면 stash를 사용한다. 아래 명령을 순서대로 적용하면 된다.

```
$> git stash
$> git pull --rebase
$> git stash pop
```