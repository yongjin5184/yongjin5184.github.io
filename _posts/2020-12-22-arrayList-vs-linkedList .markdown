---
layout: post
title:  "ArrayList 와 LinkedList 비교"
date:   2020-12-22 08:01:21 +0900
categories: java
---


- ArrayList
    * 내부적으로 데이터를 **배열**에서 관리한다. 따라서 데이터는 각각 Index 를 가지고 있다.
    * 데이터가 중간에 삽입되거나 삭제될 경우, ArrayList 는 임시배열을 생성해 데이터를 복사한 후, 인덱스를 변경하는 작업을 진행한다.
    따라서, 데이터 복사에 의한 성능 저하가 일어날 수 있다.
    * 반면, 데이터의 검색에는 유리한데 이는 인덱스를 가지고 있기 때문에 O(1) 에 해당 데이터에 접근할 수 있다.
    * ArrayList 클래스를 생성할 때는 배열의 초기 크기를 지정할 수 있다.
     ```java
      List<String> names = new ArrayList(1000);
     ```
    * 크기를 지정하지 않으면 기본 배열의 크기는 10 이다.
    * 원소로 가득한 배열에 새로운 원소를 추가할 때마다 ArrayList 클래스는 자동으로 더 큰 배열을 재할당한다. 
    이는, 시간이 소요되며 더 큰 메모리 공간을 소모한다.
    따라서, ArrayList 클래스를 생성할 때 크기가 큰 컬렉션을 이용할 거라면 기본 크기를 크게 잡는 것이 좋다.
    그렇게 하면, 리스트 크기를 키우느라 배열을 재할당하는 작업을 줄일 수 있다.

- LinkedList
    * 내부적으로 **연결 리스트**를 이용하여 데이터를 저장한다. 따라서 데이터는 Node(Date/Link) 로 연결된 상태로 관리된다.
    * LinkedList 인스턴스는 Element 라는 리스트의 첫부분을 가리키는 head 라는 필드만 참조한다.
    ```java
      public class SimpleLinkedList<E> {
          private static class Element<E> {
              E value;
              Element<E> next;
          }
          
          private Element<E> head;
      }
    ```
    
   * 따라서, 인덱스를 이용해 원소에 접근할 경우, 인덱스에 접근할 때 까지 리스트를 순회해야 한다. 최악의 경우 O(n) 의 시간이 소요된다.
    
- 언제 사용해야 하는가
    * 일반적으로 원소에 랜덤 접근할 수 있어야 하거나 리스트 크기가 클수록 ArrayList 클래스를 사용하면 좋다.
    * 리스트의 첫 부분이나 중간에 원소를 삽입/삭제할 일이 많다면 LinkedList 클래스를 사용하는 것이 좋다. 이유는 LinkedList 가 ArrayList 클래스에서 배열 재할당 과정으로 인해 발생하는 손실을 막아주기 때문이다.
    * Stack 과 같은 자료구조에도 LinkedList 를 쓰는 것이 좋다. 리스트의 첫 부분에도 원소를 간단하게 넣고 뺄 수 있기 때문이다.
    

- 참고
    * [JAVA 프로그래밍 면접 이렇게 준비한다](https://www.hanbit.co.kr/store/books/look.php?p_code=B7155705626)
