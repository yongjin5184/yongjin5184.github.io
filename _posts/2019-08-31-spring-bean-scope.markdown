---
layout: post
title:  "Spring bean scope"
date:   2019-08-31 13:48:13 +0900
categories: java
---

## Spring Bean scope 란?

  > 스프링이 관리하는 오브젝트 즉, 빈이 생성되고 존재하고 적용되는 범위
  
  - 싱글톤 
  
    1) 기본 스코프.
  
    2) 애플리케이션 컨텍스트마다 빈 오브젝트는 하나.
  
    3) 컨테이너가 존재하는 동안 유지.
    
    4) 여러개의 빈에서 DI 하더라도 매번 동일한 오브젝트가 주입.
    
    5) getBean() 메소드를 통해 DL를 하더라도 매번 동일한 오브젝트가 리턴됨이 보장.
  
  - 프로토 타입
  
    1) 하나의 빈 설정으로 여러 개의 오브젝트를 만들어서 사용
    
    2) 컨테이너가 초기 생성 시 관여, 그 후는 신경 쓰지 않음. 따라서 빈 오브젝트의 관리는 DI 받은 오브젝트에 달려 있음.
 
    3) 매번 새로운 오브젝트가 필요하면서 DI를 통해 다른 빈을 사용할 수 있어야 한다면 선택.
    
    4) 사용자의 요청별로 독립적인 정보나 작업 상태를 저장해둘 오브젝트를 만들 필요시.
    
    여기서 드는 의문점이 하나 있습니다. 
    
    > 대부분의 스프링 빈은 싱글톤으로 만들어지는데, 그렇다면 언제 프로토타입 빈을 사용하는 걸까?
    
    의문을 해소하기 위해, **토비의 스프링** >> [1.3 프로토 타입과 스코프] 에 있는 내용을 공부해 보았습니다. <br>(자세한 내용은 꼭 책을 참고하세요!)
    
    콜센터에서 고객의 A/S 신청을 받아서 접수하는 기능을 만든다고 생각해봅시다.
    
    폼 정보를 담는 오브젝트 > 서비스 계층으로 전달 > A/S신청 접수 > Email 발송의 순서로 로직이 작성될 것입니다.
    
    ```java
    public class ServiceRequest {
        String customerNo;
        String productNo;      
        String description;
    }

    public void serviceRequestFormSubmit(HttpServletRequest request) {
        ServiceRequest serviceRequest = new ServiceRequest();
        serviceRequest.setCustomerNo(request.getParameter("custno"));
        ...
        this.serviceRequestService.addNewServiceRequest(serviceRequest);
        ...
    }
    
    public void addNewServiceRequest(ServiceRequest serviceRequest) {
        Customer customer = this.customerDao.findCustomerByNo(serviceReqeust.getCustomerNo());
        this.serviceRequestDao.add(serviceRequest, customer);
    
        this.emailService.sendEmail(customer.getEmail(), "정상적으로 처리되었습니다.");
    }
    ```
    이렇게 작성된 코드를 토비의 스프링에선 아래와 같이 이야기 하고 있습니다.
    
    > 전형적인 데이터 중심의 아키텍처가 만들어내는 구조다. 즉, 도메인 모델을 반영하고 있다고 보기 힘들다.
    모델 관점으로 보자면 서비스 요청 클래스인 ServiceRequest는 Customer라는 고객 클래스와 연결되어 있어야지, 
    폼에서 어떻게 입력받는지에 따라 달라지는 customerNo나 customerId 같은 값에 의존하고 있으면 안된다.  
    
    이에 따라 **도메인 모델**을 반영해서 수정해 봅시다.
    
    ```java
    public class ServiceRequest {
        Customer customer;
        String productNo;      
        String description;
    }
        
    public void addNewServiceRequest(ServiceRequest serviceRequest) {
        this.serviceRequestDao.add(serviceRequest);
        this.emailService.sendEmail(serviceRequest.getCustomer().getEmail(), "정상적으로 처리되었습니다.");
    }
    ```
    
    여기서, ServiceRequest 자신이 CustomerDao를 사용해 Customer를 찾도록 하는 것이 핵심입니다.
    <br>
    <br>
    **프로토타입**의 등장! 
    ```java
    @Scope("prototype")
    public class ServiceRequest {
        
        Customer customer;
    
        @Autowired
        CustomerDao customerDao;
        
        public void setCustomerByCustomerNo(String customerNo){
            this.customer = customerDao.findCustomerByNo(customerNo);
        }
    }
    ```
    > DI를 적용하려면 결국 컨테이너에 오브젝트 생성을 맡겨야 한다. 또한 컨테이너가 만드는 빈이지만 매번 같은 오브젝트를 돌려주는 것이 아니라 
    new로 생성하듯이 새로운 오브젝트가 만들어지게 해야한다. 바로 **프로토타입** 스코프 빈이 필요한 때다.
    
    <br>
    컨트롤러에는 ServiceRequest 오브젝트를 new로 생성하는 대신 프로토타입으로 선언된 serviceRequest 빈을 가져오게 하면 됩니다.
    
    ```java
    @Autowired 
    ApplicationContext context;

    public void serviceRequestFormSubmit(HttpservletRequest request){
        ServiceReqeust serviceRequest = this.context.getBean(ServiceRequest.class);
        serviceReqeust.setCustomerByCustomerNo(reqeust.getParameter("custno"));
    }
    ```
    
    저 역시도 프로토타입을 위와 같이 사용하지 않았던 걸 보면, 도메인 관점에서 코드를 작성하지 못한 것 같습니다.
    깊이 **반성**하고, 다시 **공부**하고, **리펙토링** 해야겠습니다. :)
     
  
  - 스코프 
    
    > 싱글톤과 다르게 독립적인 상태를 저장해두고 사용하는 데 필요.
    
    - Request : 하나의 웹 요청 안에서 만들어지고 해당 요청이 끝날 때 제거.
    
    - Session : 하나의 HTTP Session 안에서 만들어지고 Session 이 종료되면 제거.
    
    - Global session : 포틀릿에만 존재하는 글로벌 Session에 저장되는 빈.
    
    - Application : 서블릿 컨텍스트에 저장되는 빈 오브젝트.
