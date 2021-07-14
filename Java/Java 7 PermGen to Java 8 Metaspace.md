# Java 7 PermGen to Java 8 Metaspace

- ## Permanent Generation

Java 7기준 Permanent Generation은 힙 메모리 영역중에 하나로  
자바 애플리케이션을 실행할때 **클래스와 메소드의 메타데이터를 저장하는 영역**

Permanent Generation 영역이 꽉 찼을때 발생하고  
메모리 누수가 발생했을때 OutOfMemoryError: PermGen Space error이 발생

메모리 누수의 가장 흔한 이유중에 하나로 메모리에 로딩된 클래스와 클래스 로더가 종료될때  
이것들이 가비지 컬렉션이 되지 않을때 발생한다.

\-XX:PermSize(min), -XX:MaxPermSize(max)로 사이즈 조정한다.

- ## Metaspace

Metaspace는 JDK8 이전의 Perm 영역을 대체하는 것으로 클래스와 메소드의 메타데이터들이 저장되는 영역  
Metaspace는 native memory를 사용하기 때문에 힙 영역과는 별개의 영역에 클래스 메타 데이터를 저장하고  
메모리가 부족할 경우 이를 자동으로 늘려준다.

native memory는 프로세스에 할당되는 메모리 영역으로 C 힙과 스레드 스택도 native memory를 사용한다.

![image](https://user-images.githubusercontent.com/73928346/125647480-737891ec-9420-4044-ab35-9c695331d70f.png)


​  
\[출처 : [https://www.programmersought.com/article/4905216600/\]](https://www.programmersought.com/article/4905216600/%5D)  
​  
클래스는 최초에 모두 로딩되는 것은 아니고 필요 시점에 로딩되며,  
클래스가 로딩될 때 Metaspace에 클래스 정보가 저장된다.  
​

![image](https://user-images.githubusercontent.com/73928346/125647526-a0e34656-18a7-4ef2-95e8-3d373b49dab6.png)
  
​  
\[출처 : [https://stuefe.de/posts/metaspace/what-is-metaspace/\]](https://stuefe.de/posts/metaspace/what-is-metaspace/%5D)

소스 코드양이 많아질수록 Metaspace도 그만큼 많이 사용

JVM 옵션으로 사용했던 PermSize 와 MaxPermSize는 더이상 사용할 필요가 없으며  
이 대신에 MetaspaceSize 및 MaxMetaspaceSize가 새롭게 사용되게 되었다.  
이 두 값은 Metaspace의 기본 값을 변경하고 최대값을 제한 할 수 있다.

- #### MetaspaceSize

이 설정은 JVM이 사용하는 네이티브 메모리양을 변경하는데 사용된다.  
시스템에서 기본으로 제공되는 것보다 더 많은 메모리를 사용할 것이라고 확신할 경우 이 옵션을 사용하면 된다.

다음 옵션은 최초 GC가 발생하게하는 크기이다. (최초 Metaspace 크기아님)

```
-XX:Metaspace=256m
```

- #### MaxMetaspaceSize

Metaspace 최대 크기 이 설정은 metaspace의 최대 메모리 양을 변경하는데 사용되는데 지정하지 않으면 제한을 두지 않게 된다.  
애플리케이션을 서버에서 동작시킬때 메모리 영역을 조절하고 싶거나  
메모리 누수가 발생해서 시스템 전체의 네이티브 메모리를 사용해 버리지 않도록 하기 위해서 사용하면 된다.

만약 native 메모리가 꽉 찾는데도 애플리케이션이 메모리를 더 요구 한다면  
java.lang.OutOfMemoryError: Metadata space가 발생한다.

```
-XX:MaxMetaspaceSize=256m
```

---

- ### 왜 Perm이 제거돼고 Metaspace 영역이 추가됐을까?

Metaspace 영역은 Heap이 아닌 Native 메모리를 이용함으로서  
개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.

(Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분)

즉, 각종 메타 정보를 OS가 관리하는 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.


- ### PermGen 이 Metaspace 로 바뀌면서 생긴 장점

Java 8의 장정 줌에 하나로 OutOfMemoryError: PermGen Space error는 일어나지 않는다.

PermGen 영역이 삭제되어 heap 영역에서 사용할 수 있는 메모리가 늘어났다.  
PermGen 영역을 삭제하기 위해 존재했던 여러 복잡한 코드들이 삭제되고  
PermGen 영역을 스캔 하기 위해 소모되었던 시간이 감소되어 GC 성능이 향상 되었다.




출처: [https://sheerheart.tistory.com/entry/Java-Metaspace에-대해서](https://sheerheart.tistory.com/entry/Java-Metaspace%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C) \[Spread your wings\]  
출처: [https://starplatina.tistory.com/entry/JDK8에선-PermGen이-완전히-사라지고-Metaspace가-이를-대신-함](https://starplatina.tistory.com/entry/JDK8%EC%97%90%EC%84%A0-PermGen%EC%9D%B4-%EC%99%84%EC%A0%84%ED%9E%88-%EC%82%AC%EB%9D%BC%EC%A7%80%EA%B3%A0-Metaspace%EA%B0%80-%EC%9D%B4%EB%A5%BC-%EB%8C%80%EC%8B%A0-%ED%95%A8) \[Clean Code that Works.\]
