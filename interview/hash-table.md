## Hash Table

### Hash Function
- 임의의 길이를 갖는 데이터를 입력받아 고정된 길이의 해시값을 출력하는 함수 
- 대표적 해시 함수 : *[Division Method, Digit Folding, Multiplication Method, Universal Hashing]*
  - Division Method: 나눗셈을 사용하여 입력값을 테이블의 크기로 나누어 계산하는 방식(주소 = 입력값 % 테이블의크기) <span style="color:green">테이블의 크기를 소수로 정하고 2의 제곱수와 먼 값을 사용해야 효과가 좋다고 알려져 있음</span>
  - Digit Folding: 각 key의 문자열을 ASCII 코드로 바꾸고 값을 합한 데이터를 테이블 내의 주소로 사용하는 방식
  - Multiplication Method: 숫자로 된 key값 k와 0과 1사이의 실수 A, 보통 2의 제곱수인 m을 사용하여 다음과 같은 계산을 하는 방식 `h(k) = (kA mod 1) * m`
  - Universal Hashing: 다수의 해시함수를 만들어 집합 H에 넣어두고, 무작위로 해시함수를 선택해 해시값을 만드는 방식
> [https://steadyjay.tistory.com/18]
> 조슈아 블로크는 여러 해시 함수 성능을 조사했고 31이란 숫자를 찾아냈다. 실제로 31은 메르센 소수로 수학적으로 나쁘지 않은 선택이라고 한다. 실제로 자바 JDK에 포함된 해시코드 일부에 31이 들어가있다. 
> ``` java 
> hashCode = 31 * hashCode + (e == null ? 0: e.hashCode());D
> ```
> 소수 + 홀수 곱하는 이유는 .. 짝수를 곱하면 비트의 오른쪽 값이 0으로 채워지는데 해시코드는 다양할 수록 좋기 때문에 0이 많아지면..안되지 않을까.. 
> 참고로 lombok의 @EqualsAndHashCode는 59를 곱한다.
>> Effective Java 에서 조슈아 블로크씨가 말하길..
>> *곱한 숫자를 31로 정한 이유는 31이 홀수이면서 소수이기 때문이다. 만약 이 숫자가 짝수이고 오버플로가 발생한다면 정보를 잃게된다, 소수를 곱한 것은 명확하진 않지만 **전통적으로** 그리 해왔다*...랍니다 

> 구글은 해시 함수를 딥러닝으로 학습한 모델을 적용해 충돌을 최소화한 논문을 발표 [enter link description here](https://arxiv.org/abs/1712.01208)
> <p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bPcgwf/btrXi3LHmD9/c5TPn3nBZFfg31nxeONdTk/img.png" min-width="70%" height="300px"></p>
- 해시 함수를 통해 입력된 데이터는 완전히 새로운 모습의 데이터로 변경되기 때문에 암호화 영역에서 주요하게 사용됨(ex: SHA)
- 눈사태 효과: 입력값의 일부가 변경되면 전혀 다른 값을 출력
- 해시 충돌(*Hash Collision*): 입력 데이터의 길이가 어떻든 고정된 길이의 데이터를 출력하기 때문에 입력값이 다르더라도 같은 해시값이 발생하는 경우
<p align="center"><img src="https://miro.medium.com/max/1400/1*i5JV9RiF17ftnGDvuqVFSA.png" min-width="70%" height="300px"></p>

### Hash collision
- 원인: [비둘기집 원리](https://ko.wikipedia.org/wiki/%EB%B9%84%EB%91%98%EA%B8%B0%EC%A7%91_%EC%9B%90%EB%A6%AC)
- 적재율(*load factor*): 해시 테이블의 크기에 대한 키의 개수의 비율 (키의 개수 `K`, 해시테이블의 크기 `N` => 적재율: `K/N`)
	- 해시함수가 키들을 잘 분산해 주는지를 말하는 효율성 측정에 사용
	- 일반적으로 적재율이 증가할 수록 해시 테이블의 성능은 감소
		- java10의 경우 0.75를 넘을 경우 동적 배열처럼 해시 테이블의 공간을 재할당
		- python: 0.66, Ruby: 0.5로 설정되어 있음
- 해시 충돌이 1도 없는 해시 함수를 만드는 것은 불가능
- 따라서 해시 테이블의 충돌은 완화하는 방법으로 문제를 보완해야 함
### Hashing
- 임의의 길이의 값을 ***해시함수(Hash Function)*** 를 사용하여 고정된 크기의 값으로 변환하는 작업
### Hash Table의 특징
- Hashing을 사용해서 변환한 값을 index로 삼아 *key*: *value*로 저장하는 자료 구조
- 변환된 index를 ***버킷*** 또는 ***슬롯*** 이라는 곳에 저장하여 사용
- 임의 크기의 데이터를 고정 크기 값으로 변환하기 때문에 데이터간 비교에 용이
- 기본연산: 탐색(Search), 삽입(Insert), 삭제(Delete)
- 기존자료구조인 이진탐색트리 or 배열에 비해 굉장히 빠른 속도(***key를 통한 value값 추출***)로 탐색, 삽입, 삭제를 할 수 있는 자료구조
- key는 ***유니크*** 해야 함
- 수정가능(=mutable)
- 보통 배열로 미리 hash table 사이즈만큼 생성 후에 사용함
- ex) {python: Dictionary, Ruby: Hash, Java: Map}
<p align="center"><img src="https://media.geeksforgeeks.org/wp-content/uploads/20200609180838/HashingDataStructure-min.png" min-width="70%" height="300px"></p>
### 시간 복잡도
- 해시 테이블은 key-value가 1:1로 매핑되어 있기 때문에 검색, 삽입, 삭제 과정에서 모두 평균적으로 O(1)의 시간 복잡도를 갖는다.
- 공간복잡도는 O(N) [N=키의 개수]
- 하지만..최악의 경우(모든 버킷에서 충돌이 일어날 경우) O(n)이 될 수 있다 

### 성능 좋은 해시 함수의 특징
- 해시 함수 값 충돌 최소화
- 쉽고 빠른 연산
- 해시 테이블 전체에 해시 값이 균등하게 분포
- 사용할 키의 모든 정보를 이용하여 해싱

### 해시 충돌 완화
- 분리 연결법(*Separate Chaining*)

	- 구현원리: 1) 키의 해시 값을 계산 -> 2) 해시 값을 이용해 배열의 인덱스를 구함 -> 3) 같은 인덱스가 있으면 링크드 리스트로 연결
	- 분리 연결법은 한 버킷(슬롯)당 들어갈 수 있는 엔트리 수에 제한을 두지 않는다. 해당 버킷에 링크드 리스트(linked list) 혹은 트리(tree)자료 구조를 사용한다.
	- 기본적인 자료구조와 임의로 정한 간단한 알고리즘만 존재하면 되기 때문에 인기가 많은 방법, 가장 전통적인 방법으로 흔히 해시 테이블이라고 하면 이 방식을 뜻함
	- 해시 충돌이 일어 나더라도 설정된 자료 구조로 노드가 연결되기 때문에 index가 변하지 않고 데이터 개수의 제약이 없다는 장점이 있다.
	- 하지만 데이터가 증가하면서 동일한 버킷에 연결된 노드 들이 많이지게 되면 그에 따라 캐시의 효율성이 감소한다.(검색 쏠림 현상) 
	- 추가적인 메모리를 사용
<p align="center"><img src="https://velog.velcdn.com/images/ddaogi/post/1a16afd9-5245-4568-804a-52c9f6fe0bb2/image.png" min-width="70%" height="300px"></p>
	> java 8에서는 Self-balancing Binary Search Tree 를 사용하여 연결 방식을 구현

- 개방 주소법(*Open Addressing*)
	- 추가적인 메모리를 사용하는 분리연결법과는 다르게 비어있는 해시테이블의 버킷을 활용하는 방법
	- 대표적인 방법: *[선형 탐색, 제곱 탐색, 이중 해시]*
		- 선형 탐색(Linear Probing): 충돌이 발생한 버킷의 index로 부터 **고정폭** 만큼 이동하여 차례대로 빈 버킷에 데이터를 저장하는 방식
		  > 선형 탐색은 데이터들이 고르게 분포되지 않고 **뭉치는 경향**이 있다. 해시 테이블에 연속된 데이터 그룹이 생기는 현상을 *클러스터링*이라고 하는데, 클러스터들이 커지면 인근 클러스터들과 서로 합치는 일이 발생한다. 이러면 해시 테이블의 특정 위치에 데이터가 몰리게 되고, 다른 위치에서는 상대적으로 데이터가 거의 없는 상태가 될 수 있다. 이런 ***클러스터링 현상은 시간이 오래 걸리게 되며, 해싱 효율을 떨어트리는 원인*** 이 된다. 
		- 제곱 탐색(Quadratic Probing): 저장 순서 폭을 제곱으로 저장하는 방식 예를 들어 처음 충돌이 발생시 1만큼 이동하고 계속 충돌이 발생하면 2^2,  3^2 칸씩 이동하여 저장하는 방식
		- 이중 해시(Double Hashing): 해시된 값을 한번 더 해싱하여 해시의 규칙성을 없애버리는 방식, 해시된 값을 한번더 해싱하기 때문에 다른 방법들보다 많은 연산을 함
	- 추가적인 메모리를 사용하지 않고, 추가적인 작업 없이 해시테이블 내에 데이터 저장 및 처리가능
	- 추가적인 메모리를 사용하지 않기 때문에 해시 함수의 전체 버킷의 개수 이상을 저장할 수 없음(*데이터가 늘어나면 그에 해당하는 저장소가 필요함*)
	- 테이블 내에 빈 버킷을 찾아 해결하는 방식이기 때문에 모든 원소가 **반드시 자신의 해시값과 일치하는 주소에 저장된다는 보장이 없다**.
	- *개방 주소법에서 데이터 삭제시 삭제된 공간은 **Dummy Space**로 활용되기 때문에 테이블을 **재정리** 해주는 작업 필요*
> Python Hash Table
> 파이썬의 해시테이블은 딕셔너리이다. 파이썬의 해시테이블은 충돌시 개방주소법 방식으로 구현되었다. 파이썬이 분리 연결법을 사용하지 않는 이유로 연결 리스트를 만들기 위해 추가 메모리 할당이 필요하고, 추가 메모리 할당은 느린 작업이기 때문에 택하지 않았다고 한다 <p align="center"><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/b1e9YW/btrXdNcUvXg/PUA1BePMcO4Z2hPY5lc6k1/img.png" min-width="70%" height="300px"></p>
> 
> 선형탐색 방법이 일반적으로 분리 연결 방식에 비해 성능이 더 좋다 하지만 버킷의 80%가 넘어가면 급격한 성능 저하가 발생한다. 따라서 최근 파이썬, 루비 언어들은 개방 주소방법을 택해 성능을 높이는 대신에 적재율을 낮게 잡아 성능 저하 문제를 해결하였다.
> > C++: 분리 연결법
> 자바: 분리 연결법
> 고(Go): 분리 연결법
> 루비: 개방 주소법
> 파이썬: 개방 주소법

## JAVA 의 HashMap vs HashTable
- Hash Table Code
```java 
/**  
 * Maps the specified {@code key} to the specified  
 * {@code value} in this hashtable. Neither the key nor the  
 * value can be {@code null}. <p>  
 * * The value can be retrieved by calling the {@code get} method  
 * with a key that is equal to the original key. * * @param key the hashtable key  
 * @param value the value  
 * @return the previous value of the specified key in this hashtable,  
 *             or {@code null} if it did not have one  
 * @exception NullPointerException  if the key or value is  
 *               {@code null}  
 * @see Object#equals(Object)  
 * @see #get(Object)  
 */public synchronized V put(K key, V value) {  
    // Make sure the value is not null  
  if (value == null) {  
        throw new NullPointerException();  
    }  
  
    // Makes sure the key is not already in the hashtable.  
  Entry<?,?> tab[] = table;  
    int hash = key.hashCode();  
    int index = (hash & 0x7FFFFFFF) % tab.length;  
    @SuppressWarnings("unchecked")  
    Entry<K,V> entry = (Entry<K,V>)tab[index];  
    for(; entry != null ; entry = entry.next) {  
        if ((entry.hash == hash) && entry.key.equals(key)) {  
            V old = entry.value;  
            entry.value = value;  
            return old;  
        }  
    }  
  
    addEntry(hash, key, value, index);  
    return null;  
}
```
- HashMap Code
```java
/**  
 * Associates the specified value with the specified key in this map. * If the map previously contained a mapping for the key, the old * value is replaced. * * @param key key with which the specified value is to be associated  
 * @param value value to be associated with the specified key  
 * @return the previous value associated with {@code key}, or  
 *         {@code null} if there was no mapping for {@code key}.  
 *         (A {@code null} return can also indicate that the map  
 *         previously associated {@code null} with {@code key}.)  
 */public V put(K key, V value) {  
    return putVal(hash(key), key, value, false, true);  
}
```
해시테이블의 put은 해시맵의 put과 다르게 synchronized 키워드가 존재한다. 이는 병령 프로그래밍시 동기화를 지원해준다는 것을 의미하는데.. 이는 해당 함수를 처리하는 시간이 지연됨을 의미한다.
때문에 병렬처리를 하면서 자원의 동기화를 고려해야 하는 상황이라면 해시테이블을, 병렬처리를 하지 않고 동기화를 고려하지 않는 상황이라면 해시맵을 사용하면 된다..라고 많이들 나와있지만.. Hash Table은 쓰레드간 락을 걸어서 성능의 저하가 많이 발생할 수 있기 때문에.. HashMap의 동기화 문제를 해결하기 위해 나온 ConcurrentHashMap을 사용하는 편이 더 나아보인다. ConcurrentHashMap은 Entry아이템 간 락을 걸기 때문에 Hash Table보다 데이터를 다루는 속도가 더 빠르다.
 | |`HashMap`|`Hash Table`|`Concurrent HashMap`|
 |---|:------:|:-----:|:-----:| 
 |key와 value에 null 허용|O|X|X|
 |동기화 보장(Thread-safe)|X|O|O|
 |추천 환경|싱글 쓰레드|멀티 쓰레드|멀티 쓰레드
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTIxNzIxNzgzLC0xOTQzMDgzNDMsLTIwMD
IyNzU0ODMsMTc5Njk2NzIzMSwtMTE4MDU0ODAwLC00NzA1OTk4
MTAsODQyNDA0NjI0LDMyMzE0NTgxMiwtNzA1NDYwNzk5LDIzMD
UyOTY2MSw2MzM3MTEzOTYsNzM1NjAyMjEyLDExODAxOTE4NjYs
LTE1NjYzNjc5MjcsLTEyNzUzNzM5MDcsNTU5MDQxNTcxLDEwNz
I0ODE1MDMsMTExNzA5NDA1OSwyMzYzODY5MDUsMTA4ODgxMTgy
Nl19
-->