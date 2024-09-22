---
title: "Git Merging 과 Rebase 의 상황별 사용법"
author: elegantcoder
date: 2015-05-20T11:49:12
publishDate: 2015-05-20T11:49:12
summary: "Git 을 사용하기 시작한지가 벌써 3년이다. 한번은 문상환님하고 Git 의 Merge branch 커밋에 대해 이야기를 한 적이 있다. 대화가 진행될 수록 Rebase 와 Merge 를 머리로만 알고 있을 뿐, 제대로 이해하지 못하단 걸 알게됐다. 일종의 산파법이랄까. Git 에서 코드를 합치는 방법에 대해 탕수육의 뿌먹파와 찍먹파처럼 Rebase 파와 Merging 파가 있다. 나는 Merging 파였다. Rebase 에 [&hellip;]
"
url: "/posts/git-merge-or-rebase"
titleImage: "mergerebase.png"
draft: false
lastmod: 2016-06-10T05:05:00
isCJKLanguage: true
categories:
  - dev
tags:
  - Git
  - Git Merging
  - Git Rebase
  - 번역
  - outdated
params:
  images:
    - "mergerebase.png"
---
Git 을 사용하기 시작한지가 벌써 3년이다. 한번은 [문상환](https://sangwhan.com/)님하고 Git 의 Merge branch 커밋에 대해 이야기를 한 적이 있다. 대화가 진행될 수록 Rebase 와 Merge 를 머리로만 알고 있을 뿐, 제대로 이해하지 못하단 걸 알게됐다. 일종의 산파법이랄까.

Git 에서 코드를 합치는 방법에 대해 탕수육의 뿌먹파와 찍먹파처럼 Rebase 파와 Merging 파가 있다. 나는 Merging 파였다. Rebase 에 대해서 알고는 있지만 그저 “충돌나면 머리아프니 안써” 라며 배제 했었는데 좋은 글을 찾아 요약하게 되었다.

개인적으로 중요한 내용인, 각각의 장단점과 활용방법에 대해서만 요약했다. 원문은 [소스트리 블로그](http://blog.sourcetreeapp.com/2012/08/21/merge-or-rebase/) 에서 볼 수 있다.

각 기능의 장단점
---------

### Merging 장점

-   이해하기 쉬움
-   원래 브랜치의 컨텍스트를 유지함.
-   Fast-Forward Merge 를 하지 않는다면 브랜치 별로 커밋을 분리해 유지. 특히 이런 분리는 기능 브랜치에 유용.
-   원래 브랜치의 커밋들은 변경되지 않고 계속 유지되어 다른 개발자들의 작업과 공유되는 것에 대해 신경쓸 필요가 없음.

### Merging 단점

-   단순히 모든 사람들이 같은 브랜치에서 작업하기 때문에 머지해야할 때는 merge 가 커밋 히스토리상으로 전혀 유용하지 않고 어지럽기만 하다.

### Rebase 장점

-   단순한 히스토리
-   여러 개발자들이 같은 브랜치를 공유할 때 커밋을 합치는 가장 직관적이고 깔끔한 방법.

### Rebase 단점

-   충돌상황에서 다소 복잡. 커밋 순서대로 Rebase 를 하는데, 각 커밋마다 충돌해소를 순서대로 해주어야 한다. SourceTree 가 가이드하기는 하지만, 역시 복잡한 것은 사실이다.
-   해당 커밋들을 다른 곳에 푸시한 적이 있다면 히스토리를 다시쓰는 것에 부작용이 발생한다. Mercurial 에서는 간단히 푸시를 할 수 없다. Git 에서는 Push 할 수 있으나 당신 혼자 쓰는 리모트 브랜치에만 가능하다. 만약 다른 사람이 그 브랜치를 체크아웃 받은 후 당신이 리베이스 한다면 꽤 혼란스럽게 될 것이다.

결론
--

Rebase 와 Merging 모두 나름의 가치가 있는 것으로, 논란의 대상이 아니고 상황에 따라 사용해야할 것이 다른것이다.

1.  여러 개발자들이 같은 브랜치를 공유할 때는 Pull & Rebase 가 히스토리를 깔끔하게 유지하는데 좋음.
2.  완료된 기능 브랜치를 다시 합칠 때는 머지를 사용.
3.  기능 브랜치에 부모 브랜치의 변경 내용을 반영하고 싶을 때는
    1.  아래 상황에서는 리베이스가 낫다.
        -   이 브랜치를 다른 곳에 푸시한 적 없는 경우.
        -   (Mercurial 이 아닌) Git을 사용하고 다른 사람이 이 기능브랜치를 체크아웃할 일이 없을 것이라 확신하는 경우.
    2.  이 외의 상황에는 머지가 낫다.

따라서 [KStyleTrip](http://kstyletrip.com) 는 Git 그라운드 룰을 다음처럼 정하기로 했다.

-   Remote 와 Local 에 동시에 존재하는 브랜치를 Pull 할 때에는 Rebase 를 사용하도록 한다.
-   기능 브랜치에 대해서는 Merge 를 사용, Rebase 와 비슷한 동작을 하게되는 Fast-Forward Merge 를 사용하지 않는다.
-   기능 브랜치에 그 부모 브랜치의 내용을 합칠 때는 로컬 브랜치일 때만 Rebase 로 합침.

p.s. Github 클라이언트는 Pull 시 자동으로 Rebase 한다, Sourcetree 도 해당 기능을 설정할 수 있다. 설정법은 [원문](http://blog.sourcetreeapp.com/2012/08/21/merge-or-rebase/) 참조.