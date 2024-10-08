---
title: "램디스크로 더 빠른 개발환경 구축하기"
author: elegantcoder
date: 2013-08-29T09:14:00
publishDate: 2013-08-29T09:14:00
summary: "최근에 한 웹앱 프로젝트를 완료했다. 이 프로젝트를 진행하면서 램디스크를 오랫만에 사용해보게 됐다.
웹앱은 특히 HTML템플릿을 많이 사용했는데 그 특성상 처음 퍼블리싱된 파일이 너댓개 템플릿으로 분리되는 것이 예사였다. 파일이 많아지면서 프로젝트 내에서 파일이름을 검색해 여는 단순한 작업도 은근히 느려지는 것을 느꼈다. 그리고 파일 내부 검색에도 시간이 오래걸렸다.
처음에는 SSD 로 업그레이드를 해야하는가 고민했지만 마침 얼마전 램을 16GB로 업그레이드 하면서 평소 메모리 사용량이 꽤 많이 남게 되어서 활용하기로 했다.
램드라이브를 사용하게 되면서 파일이름을 검색하든 파일 내부를 검색하든 버벅거림이 전혀 없고 아주 만족하며 사용중이다.
"
url: "/posts/ramdisk"
aliases: ["/ramdisk"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:13:32
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
최근에 한 웹앱 프로젝트를 완료했다. 이 프로젝트를 진행하면서 램디스크를 오랫만에 사용해보게 됐다.

웹앱은 특히 HTML템플릿을 많이 사용했는데 그 특성상 처음 퍼블리싱된 파일이 너댓개 템플릿으로 분리되는 것이 예사였다. 파일이 많아지면서 프로젝트 내에서 파일이름을 검색해 여는 단순한 작업도 은근히 느려지는 것을 느꼈다. 그리고 파일 내부 검색에도 시간이 오래걸렸다.

처음에는 SSD 로 업그레이드를 해야하는가 고민했지만 마침 얼마전 램을 16GB로 업그레이드 하면서 평소 메모리 사용량이 꽤 많이 남게 되어서 활용하기로 했다.

램드라이브를 사용하게 되면서 파일이름을 검색하든 파일 내부를 검색하든 버벅거림이 전혀 없고 아주 만족하며 사용중이다. 파일을 이용하는 모든 작업이 매우 빨라졌다. git 이 속도가 빠른 이유는 로컬에 저장하기 때문인데 램디스크를 사용하면 브랜치 이동 등이 더 빨라진다.

스크립트 소개
-------

아래 스크립트는 OSX 에서만 동작한다. 램디스크를 만들고, 어플리케이션을 실행하는 부분들이 특히 그렇다. 소스는 [http://github.com/elegantcoder/ramdisk](http://github.com/elegantcoder/ramdisk) 에 있다.

스크립트는 Makefile 로 되어있다. 쉘스크립트를 사용할 수도 있었겠지만 arguments 로 분리하는 것을 해본적이 없어서 섹션을 쉽게 나눌 수 있는 Makefile 을 사용했다.

타겟
--

start

램디스크를 만들고 하드디스크의 프로젝트 파일을 램디스크로 카피해둔다.

stop

make copy 를 실행하고 램디스크를 제거한다.

sync

주기적으로 make copy 를 실행한다.

apps

작업을 시작하면서 필요한 어플리케이션들을 실행하도록 했다. 나의 경우는 터미널, Sublime Text 와 Source Tree 를 실행하도록 했다.

copy

램디스크의 파일을 하드디스크에 복사하는 작업을 한다. rsync 를 사용해보기도 했는데 잠시 생성했다 지운 파일도 복사되는 경우가 있어서 파일을 삭제하고 다시 넣도록 했다. 특히 git 브랜치 이동시에 문제가 발생하는 경우가 많다. 그리고 새로운 이름으로 복사 후 이름을 바꿔주는 편이 안전하다고 생각해 그렇게 구현했다.

변수
--

새로 작성시에 변수의 마지막 슬래시에 특히 주의해야 한다.

srcPath

하드디스크 내의 소스디렉토리 이다. ex) ~/Development/project

volumeName

만들어질 램 디스크의 볼륨이름이다. 띄어쓰기 금지

targetPath

램디스크가 위치할 볼륨 디렉토리이다. 수정금지.

projectPath

램디스크내 프로젝트가 위치할 디렉토리이다. 램디스크의 root 디렉토리에는 시스템 파일등이 위치하게 되므로 파일을 복사하기가 좋지 않아 볼륨의 루트에서 한단계 아래 디렉토리에 프로젝트를 위치시켰다.

size

만들어질 램디스크의 크기이다. 단위는 섹터 수이다. 섹터는 512byte 단위이므로 아래 공식을 사용하면된다. (1GB\*1024byte^3)/512byte.

syncInterval

초단위로 싱크할 주기를 넣는다. 1200 이라면 20분에 한번꼴이다.

tempDir

파일 복사에 사용할 임시디렉토리이다. 현재 시간의 md5 해시 스트링을 사용하도록 해서 무작위로 생성되도록 했다. 굳이 수정할 필요는 없다.

참고
--

-   [OS X: How to create a RAM disk – the easy way](http://bogner.sh/2012/12/os-x-create-a-ram-disk-the-easy-way/)
