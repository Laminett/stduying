## Hash Table

### Hash Function
- 임의의 길이를 갖는 데이터를 입력받아 고정된 길이의 해시값을 출력하는 함수 
- 대표적 해시 함수 : *[Division Method, Digit Folding, Universal Hashing]*
  - Division Method: 
  - Digit Folding
  - Universal Hashing: 
- 해시 함수를 통해 입력된 데이터는 완전히 새로운 모습의 데이터로 변경되기 때문에 암호화 영역에서 주요하게 사용됨(ex: SHA)
- 눈사태 효과: 입력값의 일부가 변경되면 전혀 다른 값을 출력
- 해시 충돌(*Hash Collision*): 입력 데이터의 길이가 어떻든 고정된 길이의 데이터를 출력하기 때문에 입력값이 다르더라도 같은 해시값이 발생하는 경우
![hash collision](https://miro.medium.com/max/1400/1*i5JV9RiF17ftnGDvuqVFSA.png)
### Hash collision
- 원인: [비둘기집 원리](https://ko.wikipedia.org/wiki/%EB%B9%84%EB%91%98%EA%B8%B0%EC%A7%91_%EC%9B%90%EB%A6%AC)
- 적재율(*load factor*): 해시 테이블의 크기에 대한 키의 개수의 비율 (키의 개수 `K`, 해시테이블의 크기 `N` => 적재율: `K/N`)
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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTEyMDE1NzEsMTU4NTg1ODg2NywtMT
MzNjI1MDM2LC0xNDE3ODg5MDIsMTk0ODk0ODQ3NF19
-->