---
layout: post
title:  "Entity 생명 주기"
date:   2020-12-21 21:12:10 +0900
categories: java
---

- Entity 생명 주기
    - 비영속
    - 영속
    - 준영속
    - 삭제

    ```java
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");
    EntityManager em = emf.createEntityManager();
    EntityTransaction tx = em.getTransaction();

    // 비영속 상태
    Member member = new Member("1", "김용진");

    // 영속 상태 (쿼리가 나가지 않음 -> tx.commit() 을 해야 쿼리 실행)
    em.persist(member);

    // 준영속
    em.detach(member);

    // 삭제 (DB 에서 지우겠다.)
    em.remove(member);

    ```

    1차 캐시는 트랜잭션 단위로 만들기 때문에 여러명의 고객이 사용하는 캐시(이는 2차 캐시) 는 아니다. 
    따라서, 큰 성능의 이점이 있진 않다.

    대신, 컨셉이 주는 이점이 있다. 

    1.쓰기 지연

    ```java
    Member member1 = new Member(1, "KOBE");
    Member member2 = new Member(2, "MJ");

    // 영속성 컨텍스트에 쌓임
    em.persist(member1);
    em.persist(member2);

    // insert 쿼리가 나감
    transaction.commit();
    ```

    2.변경 감지 (Dirty Checking)

    ```java
    Member membe = em.find(Member.class, 1);
    Member.setName("LBJ");

    // 필요없음, 자바 컬렉션을 다루듯이, persist 없이도 변경이 됨
    // em.persist(member); 

    // update 쿼리가 나감
    tx.commit();

    /*
    작동방식
    1. commit 하는 시점에 내부적으로 flush 가 호출됨 
    2. entity 와 스냅샷(최초로 만들어진 값)을 비교
    3. update 쿼리를 쓰기 지연 저장소에 만들어 둠 
    4. DB 에 flush 
    5. commit
    */
    ```

    - 영속 엔티티의 동일성 보장

    ```java
    Member a = em.find(Member.class, "member1");
    Member b = em.find(Member.class, "member1");

    System.out.println(a == b); // 동일성 비교 true
    ```

    - 1차 캐시로 반복 가능한 읽기(REPEATABLE READ) 등급의 트랜잭션 격리 수준을 데이터베이스가 아닌 애플리케이션 차원에서 제공