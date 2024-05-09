## JWT
- Json 포맷을 이용하여 인증에 필요한 정보를 저장하는 Claim기반의 Web Token
  - claim은 주체사 무엇인지를 표현하는 key-value의 쌍
  - 토큰 자체가 정보를 가지는 방식
- 주로 회원인증이나 정보전달에 사용된다

### 구조
- Header, Payload, Signature의 3부분으로 이루어 짐
- 각 부분이 Json 형태로 되어있으며 Base64 url-safe 인코딩 되어 표시되고 각 부분을 이어 주기 위해 `.`구분자를 사용한다
- *Header*
  - jwt에서 사용할 토큰 타입과 해시 알고리즘의 종류가 담겨있다.
- *Payload*
  - 서버에서 첨부한 사용자 권한 정보와 데이터가 담겨있다.
- *Signature*
  - Header, Payload를 Base64 url-safe 인코딩 이후에 Header에 명시되어 있는 해시 함수를 적용하고 개인키로 서명한 전자서명이 담겨있다.

### 장단점
- 장점
  - header, payload를 통해 signature를 생성하기 때문에 데이터 위변조를 막을 수 있다.
  - 인증에 필요한 정보를 직접 담고 있기 때문에 인증을 위해 인증정보를 세션이나 DB에 저장하지 않아도 된다.
  - 서버에 인증관련 부분을 저장하지 않아도 되어 서버확정성이 높아진다.
- 단점
  - JWT토큰의 길이가 길어지면 네트워크 대역폭 낭비가 심해질 수 있다..(트래픽이 많은 서비스겠죠..?)
  - 한번 발급된 토큰은 수정, 폐기가 불가능
  - 토큰을 누구나 열람이 가능