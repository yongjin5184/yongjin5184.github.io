---
layout: post
title:  "[Design Pattern] Template Method"
date:   2019-02-18 22:25:58 +0900
categories: java
---

## 상황

  * 최근 빌링 개발팀에서는 ERP 시스템을 연동하는 과정에서 판매패키지(온라인 상품)와 물류(실물상품. ex.교재)를 ERP 형식에 맞게
  변환하여 API 전송을 해야하는 이슈가 있었습니다.
  
  * 위의 기능을 하는 코드는 이미 구현이 되어 있었고, 저는 여기에 몇 가지 조건에 따른 기능을 넣는 작업을 개발했는데, 이때 
   리펙토링을 고려하여 공부했던 Template Method 패턴을 공유하고자 합니다.

