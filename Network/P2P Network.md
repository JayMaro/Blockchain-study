# P2P Network

## Bitcoin 통신 방법
- Node와 Client(Wallet)의 통신 JSON RPC
  - Http 통신
- Node와 Node간에 블록와 Transaction을 주고 받는 Gossip Protocol
  - TCP 통신(양방향 통신)

기본적인 비트코인 통신 방법
- Flooding
- 거래가 발생하면 전파
- 각각의 노드는 옳은 거래인지 아닌지 모르기 때문에 검증
  - Consensus, UTXO

### Node와 Client(Wallet)의 통신
- JSON RPC
  - 블록체인 데이터 조회
    - getblock, gettransaction
    - Block 조회, 거래 조회
  - Wallet 관리
    - ImportPrivKey, GetBalance
    - 개인 Node에서 개인키 수신, 사용가능한 잔액 확인
  - Transaction 생성
    - sendtoaddress, signrawtransactionwitwallet
    - 서명 없는 거래 내용 전달, 서명된 거래 내용 전달

### Node와 Node간의 통신
#### 최초 네트워크 참여
1. DNS Address 에서 주소를 찾는다.
   - seed.bitcoin.sipa.be
   - dnsseed.bluematt.me
   - dnsseed.bitcoin.dashjr.org
   - etc
   - 중앙화된 서비스에서 제공하는 주소기 때문에 동작하지 않을 수 있다.
2. 해당 주소로 연결 시도(version/verack)
3. 실패시 소스코드 내의 하드코딩된 DNS Address에 연결 시도
4. 첫 노드 연결 성공시 해당 노드가 가지고 있는 노드 IP 리스트 수신
   - 메인넷은 700여개, 테스트넷은 10개의 주소 리스트 보유

#### Block Sync
##### Light weight node
- Ping/Pong
  - 노드간 통신 확인
- Header Download
  - 거래 정보가 없는 Block Header들을 받음
##### Full node
- Ping/Pong
    - 노드간 통신 확인
- Header Download
    - 거래 정보가 없는 Block Header들을 받음
- Block Download
- Block Validation

#### Block 검증
header값 체크
1. 신규 블록 수신
2. 블록 구조 일치 여부
3. Block Header의 Hash 값 비교
   - nonce 값(pow의 결과) 비교
4. Block timestamp 의 시간 체크
5. Block Size 체크
---
거래 검증
6. 채굴자의 보상에 대한 내용 확인(Coinbase Transaction Check)
7. Tx Check
---
데이터 베이스 업데이트
8. Mempool update
9. LevelDB에 새로운 블록 추가
10. Block 전파


#### Transaction 전파
1. Tx를 다른 노드에게서 전파 받음
2. 이미 받은 Tx인지 확인
3. 없는 경우 다른 노드에게 전파
   - inv(msg_tx)
4. 상대 노드가 전파받은 거래가 없는 경우 getdata를 요청
5. 새로운 거래 전달
6. 연결된 모든 Node에게 전달될 때까지 수행

#### Transaction 검증
1. 신규 Tx 수신
2. Tx 구조 일치 여부 확인
3. In, Out List 존재 여부 확인
4. Tx 크기 확인
5. Output 값 확인(< 2100만)
6. Mempool 존재 여부 확인
7. Block 존재 여부 확인(이미 승인된 거래)
8. Input Check(이중 지불 확인)
9. Input Check(이미 처리되었지만 승인되지 않은 거래인지 확인)
10. Input Check(채굴자 보상 Tx인지 확인)
    - 일정 수 이상의 Block 이후에 사용 가능
11. Input Check(UTXO가 아닌 경우)
12. Input > Output
13. Input Script Check(서명 확인)
14. Add Mempool
15. Tx 전파

#### Block 전파
1. Ping/Pong
2. Block을 전달 받음
3. 전달 받은 Block Header를 다른 Node에게 전달
4. 전달 받은 Node가 없는 Block인 경우 해당 Node에서 getdata와 headers를 요청
   - 자신의 Block이 없는 만큼 전달받기 위함
5. 새로운 Block과 그 사이 Block을 노드에게 전달

## 참고한 자료
한 번에 끝내는 블록체인 개발 A to Z
