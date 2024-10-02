---
title: "VirtualBox – Vagrant Disk IO 성능개선 방법"
author: elegantcoder
date: 2014-04-26T08:02:40
publishDate: 2014-04-26T08:02:40
summary: "* VirtualBox 에서 제공하는 공유폴더 기능은 심각한 퍼포먼스 저하가 있다.
"
url: "/posts/virtualbox-vagrant-disk-io-성능개선-방법"
aliases: ["/virtualbox-vagrant-disk-io-성능개선-방법"]
titleImage: "undefined"
draft: false
lastmod: 2015-07-24T04:12:39
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
-   VirtualBox 에서 제공하는 공유폴더 기능은 심각한 퍼포먼스 저하가 있다. 따라서 다른 방법으로 파일을 공유하는 것이 성능에 훨씬 유리하다.
    
    -   참고: Vagrant 개발자인 Mitchell Hashimoto 의 VM 내 파일시스템 성능비교 [https://mitchellh.com/comparing-filesystem-performance-in-virtual-machines](http://mitchellh.com/comparing-filesystem-performance-in-virtual-machines)
        
    -   해결법 – shared folder 를 NFS 로 공유하기 또는 Rsync 로 공유하기 (Rsync는 Vagrant 1.5 이상에서 동작)
        
        -   NFS 로 만드는 방법 간단요약 – 참고: [https://coderwall.com/p/uaohzg](https://coderwall.com/p/uaohzg) [https://docs.vagrantup.com/v2/synced-folders/nfs.html](https://docs.vagrantup.com/v2/synced-folders/nfs.html)
            
            -   Host only Adapter 추가 – Virtualbox > VM
                
            -   private\_network 행 추가 – Vagrantfile
                
            -   shared\_folder 에 type: nfs 추가 – Vagrantfile
                
        -   Host OS 의 `/etc/exports` (NFS 설정 파일) 에 Vagrant 가 자동으로 행을 추가하기 때문에 `vagrant up` 도중 sudoer 패스워드 물어봄
            
    -   Rsync 는 일반적인 Rsync 와 동일하게 동작한다. Guest 와 Host 양쪽에 `rsync` 명령어가 있으면 활용이 가능하다. 윈도우에서도 msysgit, MinGw, Cygwin 에서 Rsync 기능 제공 중
        
        -   다른 shared folder 타입들과 달리 rsync 명령어를 활용하기 때문에 Rsync 는 `vagrant up` 때 한번 싱크. 따라서 자동으로 파일 변화를 감지해 싱크를 맞춰주는 `vagrant rsync-auto` 를 사용해야 함.
