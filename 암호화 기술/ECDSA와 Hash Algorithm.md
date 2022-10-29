# Bitcoin 암호화
- 익명성
  - 신원을 드러내지 않고(Address 이용) 거래가 가능
- 부인방지
  - 본인이 보유한 개인키로 서명하기 때문에, 부인방지의 기능
- 위변조 방지
  - Hash 알고리즘과 PKI를 사용하여 거래 위변조를 방지

## ECDSA
- RSA와 동일한 암호화 서명 알고리즘
- 타원곡선 기반

### ECC
- ECC(Elliptic Curve Cryptography)는 공개키 암호기술 구현 방식 중 하나이다.
- RSA에 비해 더 작은 데이터로 RSA와 비슷한 보안성능을 제공한다.
  - 2048 -> 256
- 실제 디지털 서명방식으로 구현된 알고리즘을 ECDSA라고 부른다.
  - 디지털 서명이란 개인키로 암호화하는 것을 뜻한다.
- Bitcoin에서는 secp256k1 이라는 타원곡선을 이용한다.
  - 𝑦^2 = 𝑥^3 + 7 
- 이를 유한한 공간에서 표현하기 위해서 mod p를 통해 갈루아 필드상에서 표시한다.
  - 𝑦^2𝑚𝑜𝑑𝑝= (𝑥^3+𝑎𝑥+𝑏)𝑚𝑜𝑑𝑝

### PKI
- ECC에서 곡선 위의 점 P1, P2를 선택하면 우리는 이를 직선으로 연결하면 P3를 찾을 수 있다.
   𝑃1 + 𝑃2 = 𝑃3
- 이 수식을 Doubling 이라고 칭한다.
- Private Key는 P보다 작은 소수(d)이다.
- PublicKey는 Q=d x G 이다.
  - G는 공개된 좌표값
- Q=(G+G...+G)로표현된다.

#### Bitcoin Private Key 생성
- 256bit 길이의 랜덤 숫자를 선택: d(private key)
#### Bitcoin Address 생성
- 선택된 값을 통해 ECDSA로 public key 생성
- 생성된 값을 Hash
- Base58 Encode를 통해 생성

#### Bitcoin 거래 서명
- 거래 발생: tx -> 
- 거래 해싱: SHA256(SHA256(Tx)): msg -> 
- 개인키로 서명: ECDSA(private key, msg): sign data ->
- 공개키로 복호화: ECDSA(public key, msg): msg
- 다른 거래의 해시값과 비교

##### 서명 방식
- 개인키 d, Random 수: r, 공개키: Q(dG), 전송 거래 데이터: m
- 개인키로 서명하는 법
  - S = hash(m, rG)d + r
- 서명 검증하는 법
  - hash(m`, rG)Q + rG = SG

## Hash Algorithm
- Hash Algorithm과 가장 유사한 수학적 공식은 mod 함수이다.
  - y = x(mod n)
- 단방향 알고리즘(One-Way)
  - RSA 암호화 알고리즘은 암호화 후 복호화를 통해 데이터를 확인할 수 있지만 Hash는 확인할 수 없다.
- Collision이 거의 발생하지 않는다.
  - 수학적으로 증명되었다고 한다.


### Merkle Tree 위변조
![Merkle%20Tree%20위변조.png](Merkle%20Tree%20위변조.png)
- 각각의 거래를 해싱한다.
- 해싱한 값들로 짝을 지어 다시 해싱한다.
  - 만약 하나라면 복사해서 진행한다.
- 최상위의 해시값 하나만 저장한다.
- 만약 거래가 변조되어 해시값이 변경된다면 Merkle Root의 값이 다르기 때문에 문제가 생긴것을 알 수 있다.