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
> ![deep learning hash map structures](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/bPcgwf/btrXi3LHmD9/c5TPn3nBZFfg31nxeONdTk/img.png)* 
- 해시 함수를 통해 입력된 데이터는 완전히 새로운 모습의 데이터로 변경되기 때문에 암호화 영역에서 주요하게 사용됨(ex: SHA)
- 눈사태 효과: 입력값의 일부가 변경되면 전혀 다른 값을 출력
- 해시 충돌(*Hash Collision*): 입력 데이터의 길이가 어떻든 고정된 길이의 데이터를 출력하기 때문에 입력값이 다르더라도 같은 해시값이 발생하는 경우
![hash collision](https://miro.medium.com/max/1400/1*i5JV9RiF17ftnGDvuqVFSA.png)
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

![hash table](https://media.geeksforgeeks.org/wp-content/uploads/20200609180838/HashingDataStructure-min.png)
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
	> java 8에서는 Red-Black Tree(Self-balancing Binary Search Tree)

- 개방 주소법(*Open Addressing*)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDYxNDk2Njc1LDExODAxOTE4NjYsLTE1Nj
YzNjc5MjcsLTEyNzUzNzM5MDcsNTU5MDQxNTcxLDEwNzI0ODE1
MDMsMTExNzA5NDA1OSwyMzYzODY5MDUsMTA4ODgxMTgyNiwxNT
g1ODU4ODY3LC0xMzM2MjUwMzYsLTE0MTc4ODkwMiwxOTQ4OTQ4
NDc0XX0=
-->