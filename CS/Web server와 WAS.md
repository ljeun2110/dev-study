# Web server와 WAS

- 정적컨텐츠 ? 

  요청 인자 값에 상관없이 달라지지 않는 컨텐츠 (HTML, 이미지 등)

  어느 사용자 요청이든 항상 동일한 컨텐츠

- 동적 컨텐츠 ?  

  요청 인자에 따라 바뀔 수 있는 컨텐츠 ex) Java Program




## 웹서버 ?

웹브라우저로부터 HTTP 요청을 받아 HTML 문서와 같은 정적 컨텐츠를 제공하는 프로그램

동적컨텐츠는 WAS로 전달



- 웹서버 기능
  - 기능 1)
        정적인 컨텐츠 제공
        WAS를 거치지 않고 바로 자원을 제공한다.
  - 기능 2)
        동적인 컨텐츠 제공을 위한 요청 전달
        클라이언트의 요청(Request)을 WAS에 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답, Response)한다.
        클라이언트는 일반적으로 웹 브라우저를 의미한다.
  - 
- Web Server의 예
    Ex) Apache Server, Nginx, IIS(Windows 전용 Web 서버) 등



## WAS ?

  DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 프로그램

  WAS = Web Server + Web Container




  - WAS의 기능

    클라이언트로부터 HTTP요청을 받을 수 있다.(대부분의 WAS는 웹서버를 내장)

    요청에 맞는 정적 컨텐츠를 제공할 수 있다.

    DB 조회나 다양한 로직 처리를 통해 동적 컨텐츠를 제공할 수 있다.

    여러 개의 트랜잭션 관리 기능

    

- WAS의 예
    Ex) Tomcat, JBoss, Jeus, Web Sphere 등

    

- WAS 앞 단에 웹서버를 두는 이유

  - 책임 분할을 통한 서버 부하 방지

    정적 컨텐츠는 웹서버, 동적 컨텐츠는 WAS가 담당

  - 여러 대의 WAS 연결 가능

    로드 밸런싱을 통해 WAS가 처리해야 하는 요청을 여러 WAS가 나누어 처리할 수 있도록 설정

  - 여러 대의 WAS health check

    - health check? 서버에 주기적으로 http 요청을 보내 서버의 상태를 확인

  - 보안 강화

    리버스 프록시를 통해 실제 서버를 외부에 노출하지 않을 수 있다.
    
    SSL에 대한 암복호화 처리에 Web Server를 사용
    
    
  

---
자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 을 위해 Web Server와 WAS를 분리한다.
서비스 확장성, 안정성을 고려한다면 앞 단에 웹서버를 두는 것이 유리하다.





#### reference
- https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html