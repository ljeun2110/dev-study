# DataBase Sharding

+ ### 샤딩(Sharding)이란?

  + 데이터베이스 분야에서 부하 분산 및 성능, 확장성 및 I/O 대역폭을 개선하기 위해   
    Application/DB Level에서 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법을 의미   

  + 일종의 Horizontal Partitioning

  <br>

  ![image](https://user-images.githubusercontent.com/73928346/126778046-db620b19-a599-431c-894a-c0b8c72a1a13.png)

  <br>

  + 샤드 혹은 샤딩기법은 데이터를 분리하기 용이한 기준을 잡고 데이터를 분산저장하거나   
    Row의 행수 등을 기준으로 분산저장을 한다.  

  + 샤드의 문제를 방지하기 위해서 리플리카(replica)의 방식을    
    섞어서 실제로는 둘 중 하나의 방식이 아닌 여러대의 노드에 혼합하여 처리한다.    

  + 관계형 데이터베이스에서 대량의 데이터를 처리하기 위해서 데이터를 파티셔닝하는 기술이다.   
    파티셔닝은 DBMS에서 지원하기도 하는데, 일부 DBMS ( MySQL 5.1 미만에서는 지원 안함) 에서는 지원안하기도 한다.  

  + 예)   
    전 세계의 고객 데이터를 저장하는 대형 데이터베이스를 분산한다고 할때,    
    미국 고객의 경우는 샤드A, 아시아 고객의 경우는 샤드B, 유럽 고객의 경우는 샤드C로 분할해서 저장할수 있다.    


<br>

+ ### 샤드 수행 시 고려사항
  - 데이터 재분배 : 서비스 정지 없이 데이터베이스 스키마 및 서버 설계 필요

  - 샤딩 조인 : 역정규화를 어느정도 감수해야 함

  - 샤딩 알고리즘 : 정수값 등으로 샤딩을 처리할 때 데이터의 비율 고려


  샤드 수행 시 가장 중요 한 것은 무엇보다 해당 서버의 장애가 났을 때 replica의 존재여부    
  장애처리를 전혀 하지 않고 샤딩처리만 할 경우 데이터의 크기는 분산처리한 만큼 줄어들겠지만,    
  장애가 발생할 경우 치명적이 될 수 있다.

<br>

+ ### 단점
  + Cross-Partition Query가 발생할 경우 기존의 Query보다 느릴 수 있다.   

  + 반드시 Trade-Off가 있다.   

  + Locator와 Sync해야하는 비용이 필요하다.   

  + 프로그래밍적, 운영적인 복잡도가 높아진다.   
    즉, 샤딩을 시작하기 전에 샤딩을 피하거나 지연시킬 수 있는 방법을 찾는게 우선이다.
    
    <br>
    
    **샤딩을 시작하기 전에 샤딩을 피하거나 지연시킬 수 있는 방법**
    ```
    1. 좀 더 고스펙의 컴퓨터를 사용한다.

    2. Read(Select) 명령이 많다면 Cache나 Database Replication을 적용한다.

    3. Table의 일부 Column만 사용한다면 수직 분할(Vertically Partitioning)을 사용한다 / Cold & Hot data를 사용한다.  
    ```

<br>


# Database Replication

+ ### 배경
  아주 단순한 Database를 구성할때에는 아래의 그림처럼 하나의 서버와 하나의 Database를 구성하게 된다.

  ![image](https://user-images.githubusercontent.com/73928346/126777662-b7e460a9-5a0f-4623-9fb6-516a7fb15f12.png)


  하지만 사용자는 점점 많아지고 Database는 많은 Query를 처리하기엔 너무 힘든 상황이 오게 된다.

  Query의 대부분을 차지하는 Select를 어느 정도 해결하기 위해 Replication이란 방법이 나오게 되었다.

<br>

+ ## Replication이란?
  2개의 이상의 DBMS 시스템을 Mater / Slave로 나눠서 동일한 데이터를 저장하는 방식

  ![image](https://user-images.githubusercontent.com/73928346/126772475-4076ef7b-25d6-4e92-a52d-c2f0abfdd5a4.png)

  Master DBMS에는 Insert, Update, Delete의 기능을 수행하고 Replication을 하여 Slave DBMS에 실제 데이터를 복사한다.

  Slave DB 시간이 오래걸리는 Select문의 기능을 수행하여 전체적인 Select문 성능을 향상시킨다.

<br>

+ ## 데이터를 복사하는 방법

  ### 로그기반 복제(Binary Log)
  + Statement Based : SQL문장을 복사하여 진행   
  + issue : SQL에 따라 결과가 달라지는 경우(Timestamp, UUID, …)   
  + Row Based : SQL에 따라 변경된 Row 라인만 기록하는 방식   
  + issue : 데이터가 많이 변경된 경우 데이터 커질 수 밖에 없다.   
  + Mixed : 기본적으로 Statement Based로 진행하면서 필요에 따라 Row Based를 사용한다.   

<br>

+ ## Replication 장점
  ![image](https://user-images.githubusercontent.com/73928346/126772527-92fe6609-b0af-41ee-b407-4f45415f1966.png)


  언급했던 것처럼 Query의 대부분은 Select가 차지하고 있다.   

  이 부분의 부하를 낮추기 위해 많은 Slave Database를 생성하게 된다면 Read(Select) 성능 향상 효과를 얻을 수 있다.   

  Master Database 영향없이 로그를 분석할 수 있다.   


<br>

#### reference
+ https://nesoy.github.io/articles/2018-05/Database-Shard
+ https://nesoy.github.io/articles/2018-02/Database-Replication
+ https://needjarvis.tistory.com/574
+ https://hanburn.tistory.com/106