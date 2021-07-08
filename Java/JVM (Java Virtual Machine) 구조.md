# JVM (Java Virtual Machine)
+ Java Byte Code를 OS에 맞게 해석해주는 역할
+ 일반 어플리케이션의 코드는 OS만 거치고 하드웨어로 전달되는데   
  Java 어플리케이션은 JVM을 한 번 더 거치기 때문에 하드웨어에 맞게 완전히 컴파일된 상태가 아니고     
  실행 시에 해석(interpret)되기 때문에 속도가 느리다는 단점을 가지고 있다.
+ bytecode를 interpreter 형태로 OS에 맞추어 번역, 실행하여 JAVA와 OS 사이에서 중계자 역할로
  JAVA가 OS에 구애받지 않고 재사용을 가능하게 해준다.
  
+ JVM은 크게 3부분으로 나눌 수 있는데    
  클래스 파일을 로딩한 뒤 검증하고 초기화하는 Class loader System,    
  클래스 파일을 저장하는 Runtime DataArea,   
  클래스 파일(바이트코드)를 플랫폼에 맞는 기계어로 변환시켜 실행하는 Execution engine 으로 구성
  ```
  Runtime DataArea 는 다시 method area, heap, java stacks, pc registers, native method stacks의 5가지 영역으로 나뉜다.
  ```

<br>

<img src="https://user-images.githubusercontent.com/73928346/124979372-edc37980-e06d-11eb-9f48-2985824e322c.png" width="500">

<br>

### 클래스 로더 (class loader)   
변환된 바이트 코드 파일을 JVM 내로 class를 로드하고 link 작업을 통해 배치 등 일련의 작업을 한다.   
+ 로딩: 클래스 읽어오는 과정   
+ 링크: 레퍼런스를 연결하는 과정   
+ 초기화: static 값들 초기화 및 변수에 할당   

<br>

### 실행 엔진 (execution engine)
클래스 로더를 통해 JVM 내부로 넘어와 runtime data area(JVM 메모리)에 배치된 바이트 코드들을 명령어 단위로 실행시킨다.
인터프리터: 바이트 코드를 한줄 씩 실행.
JIT 컴파일러: 인터프리터 효율을 높이기 위해, 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러로 반복되는 코드를 모두 네이티브 코드로 바꿔둔다. 그 다음부터 인터프리터는 네이티브 코드로 컴파일된 코드를 바로 사용한다.
GC(Garbage Collector): 더이상 참조되지 않는 객체를 모아서 정리한다. 어플리케이션이 생성한 객체의 생존 여부를 판단하여, 더이상 참조되지 않거나 null인 객체의 메모리를 해체시켜 메모리 반납을 한다.

<br>

### JVM 메모리
