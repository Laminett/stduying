## Thread Safe
- 멀티 쓰레드 환경에서, 공유자원에 여러 쓰레드가 동시에 접근하더라도 실행에 문제가 없는 상태

### Thread Safe를 지키기 위한 방법
- Re-entrancy (재진입성)
    - 스레드에 의해 부분적으로 실행되거나 동일한 스레드/다른 스레드에 의해 동시에 실행되면서도 원래 실행을 올바르게 완료할 수 있는 속성
    > 재진입성이 보장되는 함수의 조건
    > - 전역 변수를 사용하면 안 된다.
    > - 호출자가 호출 시 제공한 매개변수만으로 동작해야 한다.
    > -다른 비-재진입 함수를 호출하면 안 된다.
- Thread-local storage (쓰레드 지역 저장소)
    - 공유 자원의 접근을 최대한 줄여 각 스레드에서만 접근 가능한 저장소를 사용
    - ex1) 한식을 서빙할 때 각 서버들이 반찬(공유자원)을 하나씩 서빙하는 것이 아니라 쟁반(TLS)에 여러개의 반찬을 담아서 서빙
    - ex2) 호랑이가 고기를 먹을 때 크게 한덩어리를 뜯어서 자기만의 공간에 가서 먹는거랑 비슷..?
- Atomic operation (원자 연산)
    - 공유자원 접근 시 원자연산 또는 원자적으로 정의된 접근 방법을 사용해 상호 배제를 구현
      
      >- 주요 Class
      >  - AtomicBoolean
      >  - AtomicInteger
      >  - AtomicLong
      >  - AtomicIntegerArray
      >  - AtomicDoubleArray  
      >
      >사용 시 내부적으로 Compare-And-Swap(CAS) 알고리즘을 사용해 lock 없이 동기화 처리를 할 수 있습니다. Atomic Type의 compareAndSet() 메소드가 해당 역할을 수행합니다.

      >>  Compare-And-Swap(CAS): 현재 주어진 값(=현재 쓰레드에서의 데이터)과 실제 메모리에 저장된 데이터를 비교해서 두 개가 일치할때만 값을 업데이트 함
      
- Mutual exclusion (상호 배제)
    - 해당자원에 대한 접근을 세마포어/뮤텍스 와 같은 락을 통제 
    - java의 경우 synchronized를 사용하여 임계구역을 정의
      ```txt
      1. 메소드 적용방식: 메소드에 synchronized 키워드를 사용하여 적용하고 한쓰레드만 해당 메소드를 사용할 수 있습니다.
      2. 코드블럭 적용방식
            - synchronized(this): 모든 synchronized block에 락이 걸림 = 메소드 적용방식
            - synchronized(Object): 각 블록마다 다른 락이 걸리게 설정이 가능합니다
      ```
    - python의 경우 GIL(Global Interpreter Lock)을 사용해 Thread Safe를 보장
- Immutable Object (불변 객체)
    - 객체 생성 이후에 값을 변경할 수 없도록 만든다.


### 원자 연산(atomic operation)
- 다른 스레드에 의해 방해받지 않는 방법
- 런타임 라이브러리에서 사용가능한 특별한 기계언어 명령어가 필요함
- 스레드 잠금 메커니즘의 기초를 형성 (원자 연산시 잠금 설정)
- '원자적인 방법' 중간에 어떠한 방해도 받지 않고 기계 언어(ex> 어쎔블리어) 명령어 하나로 실행할 수 있는 방법
- 메모리에 접근하지 않거나, 한번 접근하는 어셈블리어 명령은 원자적이다.
