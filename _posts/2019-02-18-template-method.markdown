---
layout: post
title:  "[Design Pattern] 템플릿 메소드 패턴 Template Method"
date:   2019-02-18 22:25:58 +0900
categories: java
---

## 상황

  * 최근 빌링 개발팀에서는 ERP 시스템을 연동하는 과정에서 판매패키지(온라인 상품)와 물류(실물상품 ex.교재)를 ERP 형식에 맞게
  변환하여 API 전송을 해야 하는 이슈가 있었습니다.
  
  * 위의 기능을 하는 코드는 이미 구현이 되어 있었고, 저는 여기에 몇 가지 조건에 따른 기능을 넣는 작업을 개발했는데, 이때 
   리펙토링을 고려하여 공부했던 Template Method 패턴을 공유하고자 합니다.
   
  * 참고 서적 :  [Java 언어로 배우는 디자인 패턴 입문](http://www.yes24.com/Product/goods/2918928)

# 템플릿 메소드 패턴  

## 정의
  * 상위 클래스에서 처리의 뼈대를 결정하고, 하위 클래스에서 그 구체적인 내용을 결정하는 디자인 패턴을 **Template Method** 패턴이라고 부릅니다. 


## 장점
  * 상위 클래스의 템플릿 메소드에서 해야 할 일(기능)이 기술되어 있으므로, 하위 클래스 측에서는 해야 할 일을 다시 기술할 필요가 없습니다.
  
  * 하위 클래스를 상위 클래스와 호환이 가능하도록 구조화합니다.
  
    이는, 자바의 다형성을 활용한 패턴 중 하나로써, 
  
  > 객체지향 개발 5대 원리 SOLID 중 LSP (리스코브 치환의 원칙: The Liskov Substitution Principle) 
  >> 서브 타입은 항상 기반 타입으로 교체가 가능해야 한다. 
  
  와 맞닿아 생각해볼 수 있습니다.


## 예제코드  
### AbstractDisplay.java
  ```java
  public abstract class AbstractDisplay {
      public abstract void open();
      public abstract void print();
      public abstract void close();
      public final void display(){
          open();
          for(int i = 0; i < 5; i++){
              print();
          }
          close();
      }
  }
  ```
### CharDisplay.java
  ```java
  public class CharDisplay extends AbstractDisplay {
      private char ch;
      public CharDisplay(char ch){
          this.ch = ch;
      }
      @Override
      public void open() {
          System.out.print("<<");
      }
  
      @Override
      public void print() {
          System.out.print(ch);
      }
  
      @Override
      public void close() {
          System.out.println(">>");
      }
  }
  ```
### StringDisplay.java
  ```java
  public class StringDisplay extends AbstractDisplay{
      private String string;
      private int width;
  
      public StringDisplay(String string){
          this.string = string;
          this.width  = string.getBytes().length;
      }
      @Override
      public void open() {
          printLine();
      }
  
      @Override
      public void print() {
          System.out.println("|" + string + "|");
      }
  
      @Override
      public void close() {
          printLine();
      }
  
      public void printLine(){
          System.out.print("+");
          for(int i = 0; i < width; i++){
              System.out.print("-");
          }
          System.out.println("+");
      }
  }
  ```
### TemplateMethodExam.java
  ```java
  public class TemplateMethodExam {
      public static void main(String[] arg){
          AbstractDisplay d1 = new CharDisplay('H');
          AbstractDisplay d2 = new StringDisplay("Hello, world.");
          AbstractDisplay d3 = new StringDisplay("안녕하세요.");
  
          d1.display();
          d2.display();
          d3.display();
      }
  }
  ```
### 결과화면
    <<HHHHH>>
    +-------------+
    |Hello, world.|
    |Hello, world.|
    |Hello, world.|
    |Hello, world.|
    |Hello, world.|
    +-------------+
    +----------------+
    |안녕하세요.|
    |안녕하세요.|
    |안녕하세요.|
    |안녕하세요.|
    |안녕하세요.|
    +----------------+

## 결론
  * 이번에 공부한 Template Method 패턴은 아래와 같은 상황에 사용을 고려하면 됩니다.
  
  >  공통(중복)사항을 상위 클래스에서 정의하고 하위 클래스에서 구체적 기능을 작성할 경우
  
  