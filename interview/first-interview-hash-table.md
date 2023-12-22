## Hash Table

### Hashing
- 임의의 길이의 값을 ***해시함수(Hash Function)*** 를 사용하여 고정된 크기의 값으로 변환하는 작업
### Hash Table
- Hashing을 사용해서 변환한 값을 index로 삼아 *key*: *value*로 저장하는 자료 구조
- 임의 크기의 데이터를 고정 크기 값으로 매핑하는데 사용할 수 있으므로 데이터간 비교에 용이
- 기본연산: 탐색(Search), 삽입(Insert), 삭제(Delete)
- 기존자료구조인 이진탐색트리 or 배열에 비해 굉장히 빠른 속도(***key를 통한 value값 추출***)로 탐색, 삽입, 삭제를 할 수 있는 자료구조
- key는 유니크해야 함
- 수정가능(=mutable)
- 보통 배열로 미리 hash table 사이즈만큼 생성 후에 사용함
- ex) {python: Dictionary, Ruby: Hash, Java: Map}

![hash table](https://media.geeksforgeeks.org/wp-content/uploads/20200609180838/HashingDataStructure-min.png)
### 시간 복잡도
- 해시 테이블은 key-value가 1:1로 매핑되어 있기 때문에 검색, 삽입, 삭제 과정에서 모두 평균적으로 O(1)의 시간 복잡도를 갖는다.
- 공간복잡도는 O(N) [N=키의 개수]

### 성능 좋은 해시 함수의 특징
- 해시 함수 값 충돌 최소화
- 쉽고 빠른 연산
- 해시 테이블 전체에 해시 
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQzMDg2NzI4LC0xMzM2MjUwMzYsLTE0MT
c4ODkwMiwxOTQ4OTQ4NDc0XX0=
-->