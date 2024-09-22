---
title: "AWS MySQL Aurora 최대 접속수 수정방법"
author: elegantcoder
date: 2020-02-19T01:11:59
publishDate: 2020-02-19T01:11:59
summary: "오랜만에 Aurora MySQL의 최대 접속수를 다시 지정했다. 개발용도의 데이터베이스는 사용량은 작지만 여러 프로젝트들이 connection pool 을 생성하니 기본 정의값보다 올려서 사용하는 경우가 종종있다. max_connections를 다시 지정한 파라미터 그룹으로 교체하면 된다. 이건 TIL이라 할 것도 없다. 하지만 글로 남기는건 10분이면 될 일을 1시간이나 헤맸기 때문. 클러스터 파라미터그룹과 인스턴스 파라미터 그룹 양쪽에 max_connections가 있다. 파라미터 수정 후 [&hellip;]
"
url: "/posts/aws-mysql-aurora-최대-접속수-수정방법"
titleImage: "undefined"
draft: false
lastmod: 2020-06-30T22:49:06
isCJKLanguage: true
categories:
  - aws
  - dev
tags:
  - aws
  - TIL
  - outdated
params:
  images:
    - "undefined"
---
오랜만에 Aurora MySQL의 최대 접속수를 다시 지정했다. 개발용도의 데이터베이스는 사용량은 작지만 여러 프로젝트들이 connection pool 을 생성하니 기본 정의값보다 올려서 사용하는 경우가 종종있다. max\_connections를 다시 지정한 파라미터 그룹으로 교체하면 된다. 이건 TIL이라 할 것도 없다.

하지만 글로 남기는건 10분이면 될 일을 1시간이나 헤맸기 때문.

1.  클러스터 파라미터그룹과 인스턴스 파라미터 그룹 양쪽에 max\_connections가 있다.
2.  파라미터 수정 후 apply immediately로는 동작하지 않아서 reboot 후 적용되었다.

변경된 수치는 다음쿼리로 확인가능하다.

```
mysql> select @@max_connections;
+-------------------+
| @@max_connections |
+-------------------+
|               300 |
+-------------------+
1 row in set (0.00 sec)
```