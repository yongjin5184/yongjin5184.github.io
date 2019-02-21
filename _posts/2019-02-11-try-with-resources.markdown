---
layout: post
title:  "try-finally 보다는 try-with-resources를 사용하자"
date:   2019-02-11 16:05:58 +0900
categories: java
---

## 상황

  * 최근 빌링 개발팀에서는 파일을 읽어와 해당 파일에 있는 관리자 아이디에만 권한을 지급해야 하는 이슈가 있었습니다.
  
  * 이는 빌링 관리자의 상품 수정 페이지에서 이루어지는 작업으로 아래와 같은 코드를 작성하면 됩니다.


```java
Boolean isPermissionedUser = false;
BufferedReader bufferedReader = null;
FileInputStream fileInputStream = null;
InputStreamReader inputStreamReader = null;
try{
    fileInputStream = new FileInputStream(filePath);
    inputStreamReader = new InputStreamReader(fileInputStream);
    bufferedReader = new BufferedReader(inputStreamReader);
    String[] userIdArr = bufferedReader.readLine().replaceAll("\\s", "").trim().split(",");
    for (String id : userIdArr) {
        if(userId.equals(id)){
            isPermissionedUser = true;
            break;
        }
    }
}catch(IOException e){
    e.printStackTrace();
}finally{
    try {
        fileInputStream.close();
        inputStreamReader.close();
        bufferedReader.close();
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

해당 코드는 예외가 발생할 수 있는 IO 관련 클래스에 try catch 블록을 문제없이 사용하였고, finally 블록에 자원을 회수하는 close 메소드까지 문제없이 호출하였습니다.

따라서, 해당 코드는 아무런 문제가 없습니다.

> 하지만, 위의 코드가 최선일까요?


이펙티브 자바 Item.9 에서는 다음과 같이 이야기하고 있습니다.

> try-finally 보다는 try-with-resources를 사용하라



이유는 두 가지입니다.

### 첫째,

> 코드가 간결해집니다. 

간결한 코드가 에러를 줄이는 건 만고불변의 진리. 아래는 try-finally 구문을 try-with-resources로 바꾼 코드입니다.

```java
Boolean isPermissionedUser = false;
try( BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(filePath))) ){
    String[] userIdArr = br.readLine().replaceAll("\\s", "").trim().split(",");
    for (String id : userIdArr) {
        if(userId.equals(id)){
            isPermissionedUser = true;
            break;
        }
    }
}catch(IOException e){
    e.printStackTrace();
}
```


### 둘째,

> try-with-resources를 사용하면, 우리는 자원의 회수에 대해 신경 쓸 필요가 없습니다. 

즉, finally에서 자원을 직접 close 할 필요없이, try에 resource를 선언하여 자원 회수를 JVM에 위임할 수 있습니다.

이는 개발자가 자원 회수를 잊어 성능이 저하될 가능성이 없다는 걸 의미합니다.


* * *

## 결론,

자바 라이브러리에는 close 메서드를 호출해 직접 닫아줘야 하는 자원이 많은데 

(예를 들어 InputStream/OutputStream/java.sql.Connection 과 관련된 클래스들을 사용할 때 등)

위의 두 가지 이유로 try-with-resources가 try-finally 보다 더 나은 선택이니, JDK1.7 이상을 쓰는 서비스의 개발자라면 try-with-resources를 반드시 사용합시다.
