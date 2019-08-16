---
layout: post
title:  "Http status code를 정리해 보자"
date:   2019-08-16 18:40:58 +0900
categories: java
---

## 상황

  * HTTP status code를 정리해 보자

* * *

## Response Class

   * 상태 코드의 첫 번째 숫자

* * *
   
## 100 ~ 199 : Information responses

   > 요청을 받았으며 작업을 계속한다.
   
   - `100` : 요청자는 요청을 계속해야 한다. 첫 번째 부분을 받았으며 나머지를 기다리고 있음을 나타낸다.
   
## 200 ~ 299 : Successful responses
   
   > 요청을 성공적으로 받았으며 성공적으로 처리했음을 의미한다.
   
   - `200` **OK** : 요청 정상 처리.
   - `204` **No Contents**: 요청 정상 처리, 보낼 내용이 없다. 경우에 따라선 헤더가 유용할 수 있다.

## 300 ~ 399 : Redirection messages

   > 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다.
   
   - `301` **Moved Permanently** : 요청된 자원의 URI가 **영구적**으로 변경되었음. 새로운 URI를 응답한다.
   - `302` **Found** : 요청된 자원의 URI가 **일시적**으로 변경되었음. 향후 요청시, 옮기기 전 URI를 호출해야한다.  

## 400 ~ 499 : Client error responses
   
   > 클라이언트에 오류가 있음을 나타낸다.
   
   - `400` **Bad Request** : 클라이언트의 요청이 잘못됨.
   - `401` **Unauthorized** : 권한 없음 / 인증되지 않음. 로그인이 필요한 페이지에 해당 응답을 내려줄 수 있다.  
   - `403` **Forbidden** : 클라이언트는 컨텐츠에 대한 액세스 권한이 없다.
   - `404` **Not Found** : 클라이언트가 요청한 페이지(Resource)를 서버가 찾을 수 없다.
   - `405` **Method Not Allowed** : 클라이언트가 요청한 Method가 허용되지 않은 경우.
   
## 500 ~ 599 : Server error responses

   > 서버에 오류가 있음을 나타낸다.
    
   - `500` **Internal Server Error** : 서버에 오류가 발생하여 요청을 수행하지 못함.
   - `502` **Bad Gateway** : 서버가 **게이트웨이**나 **프록시** 역할을 수행하는 동안 Inbound Server로 부터 유효하지 않은 응답 받은 경우.
   - `503` **Service Unavailable** : 유저 관리등을 위해 일시적으로 요청을 수행하지 못함. 이 응답값과 함께 사용자 친화적인 페이지를 보내야 한다. 가능하면 HTTP Header 서비스 복구 예상 시간이 포함되어야 한다.
   - `504` **Gateway Timeout** : 서버가 게이트웨이나 프록시 역할을 수행하는 동안 Inbound Server로 부터 응답 시간이 초과된 경우. 

## 참고

   - 프록시 서버 : 클라이언트로부터 요청받은 Request를 서버로 전달하고, 서버에서 받은 Response를 클라이언트에 전달하는 역할을 한다.
   - 게이트 웨이 : 프록시와 유사(중계 역할). 클라이언트는 게이트웨이와 HTTP통신을 하지만, 게이트웨이는 서버와 HTTP 이외의 프로토콜을 사용하여 통신할 수 있다. 
                      
                     