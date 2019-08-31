---
layout: post
title:  "Spring bean scope"
date:   2019-08-31 13:48:13 +0900
categories: java
---

## Spring bean scope 란?

  > 스프링이 관리하는 오브젝트 즉, 빈이 생성되고 존재하고 적용되는 범위
  
  - 싱글톤 
  
    1) 기본 스코프
  
    2) 애플리케이션 컨텍스트마다 빈 오브젝트는 하나
  
    3) 컨테이너가 존재하는 동안 유지
    
    4) 여러개의 빈에서 DI 하더라도 매번 동일한 오브젝트가 주입
    
    5) getBean() 메소드를 통해 DL를 하더라도 매번 동일한 오브젝트가 리턴됨이 보
  
  - 프로토 타입
  
    1) 하나의 빈 설정으로 여러 개의 오브젝트를 만들어서 사용
  
  - 스코프 
  
    1) Request, Session, Global session
