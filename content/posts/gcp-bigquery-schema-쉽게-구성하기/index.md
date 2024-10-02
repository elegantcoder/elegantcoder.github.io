---
title: "GCP BigQuery Schema 구성 팁"
author: elegantcoder
date: 2020-02-12T02:25:04
publishDate: 2020-02-12T02:25:04
summary: "생성은 CSV를 통하면 편리하다. BigQuery의 테이블스키마는 생성할때 UI가 꽤 불편한 편이다. 그래서 텍스트로 편집 옵션이 있기도 하지만 타이핑하기 좋지 않은 신택스를 가지고 있어서 CSV업로드로 자동생성하는 편이 개인적으로는 가장 편했다. CSV 파일 업로드는 10MB까지만, 그 이상은 Cloud Storage 등에서 가져와야 한다. CSV가 다룰 수 있는 데이터 타입에는 한계가 있다. 예를들어 날짜시간 등을 사용하고자 하면 메뉴얼의 Limitation부분을 [&hellip;]
"
url: "/posts/gcp-bigquery-schema-쉽게-구성하기"
aliases: ["/gcp-bigquery-schema-쉽게-구성하기"]
titleImage: "undefined"
draft: false
lastmod: 2020-02-12T17:23:55
isCJKLanguage: true
categories:
  - dev
tags:
  - BigQuery
  - GCP
  - TIL
  - outdated
params:
  images:
    - "undefined"
---
생성은 CSV를 통하면 편리하다.
------------------

BigQuery의 테이블스키마는 생성할때 UI가 꽤 불편한 편이다. 그래서 텍스트로 편집 옵션이 있기도 하지만 타이핑하기 좋지 않은 신택스를 가지고 있어서 CSV업로드로 자동생성하는 편이 개인적으로는 가장 편했다.

CSV 파일 업로드는 10MB까지만, 그 이상은 Cloud Storage 등에서 가져와야 한다.

CSV가 다룰 수 있는 데이터 타입에는 한계가 있다. 예를들어 날짜시간 등을 사용하고자 하면 메뉴얼의 Limitation부분을 잘 읽어보고 활용해야한다.

테이블 변경은 쿼리결과 저장 기능으로
--------------------

BigQuery는 ALTER TABLE 문을 지원하지 않는다. 따라서 잘못만들면 테이블을 다시 만들어야 한다. 이때 테이블로 결과를 보내는 쿼리결과 저장기능을 활용할 수 있다. 다시말해 SELECT \* FROM 식으로 조회한 결과를 테이블로 보내는데, 자기테이블을 덮어씌우는 방법을 사용한다. 이때 AS를 사용해 리네임을, CAST 함수를 사용해 컬럼의 타입을 변경할 수 있다.  
   
물론 이에 따른 데이터 조회 / 쓰기 비용은 발생한다.

레코드는 피하자
--------

except() 함수 등을 활용할 때 레코드는 통째로 처리되어서 불편할때가 있다. 예를들어 http 응답을 response.status, response.body 등으로 담았다고 치자. response.body 는 덩치가 크니까 가져오지 말고 response.status만 원하면 SELECT \* except(response.body) 는 동작하지 않는다. 원하는 필드명을 하나씩 명시해야 한다.  BigQuery에서 필드 = 돈이다.

참고:  
[https://cloud.google.com/bigquery/docs/loading-data-cloud-storage-csv#appending\_to\_or\_overwriting\_a\_table\_with\_csv\_data](https://cloud.google.com/bigquery/docs/loading-data-cloud-storage-csv#appending_to_or_overwriting_a_table_with_csv_data)  
[https://cloud.google.com/bigquery/docs/manually-changing-schemas?hl=ko](https://cloud.google.com/bigquery/docs/manually-changing-schemas?hl=ko)
