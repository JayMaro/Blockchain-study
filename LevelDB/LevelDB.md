# LevelDB

Key-Value Database 
- Google에서 개발한 빠르고 가벼운 Key-Value DB
- C++ 언어로 구현됨
- File 형태로 관리되며, RDBMS와 다른 형태로 구현
- 읽기, 쓰기 성능이 빠르다.
- **Bitcoin Core file system에서 사용된다.**
  - file을 직접 저장하기 보단 file의 위치나 index를 저장하는 용도로 사용됨
- 제한점
  1) NoSQL 데이터베이스이기 때문에, 관계형 검색 불가능
  2) Single Processing : 하나의 프로세스만이 한번에 특정 데이터베이스에 접근 가능

## LevelDB 저장 구조
| Key                                            | Value                                                                                 | Desc                                    |
|------------------------------------------------|---------------------------------------------------------------------------------------|-----------------------------------------|
|'b' + 32-byte block hash                        | Block header, 블록높이, Tx 수, block Validation 여부, block Data 저 장 파일 위치, rev 데이터 저장 파일 위치 | Block Index 기록                          |
|'f' + 4-byte file number                        | 파일 내 블록 수, 블록 파일 크기, rev 파일 크기, 블록 파일 내 블록 최고 *최저 높이, 블록 파일 내 최고*최저 시간                | 파일 정보 기록                                |
|'l' -> 4-byte file number                       | 마지막 블록 파일 숫자 Blk + 0010.dat                                                           | 	                                       |
|'R' -> 1-byte boolean                           | 1(True) 0(False)                                                                      | Reindexing 여부                           |
|'F' + 1-byte flag name length + flag name string| 1(True) 0(False)                                                                      | Txindex On/Off 여부                       |
|'t' + 32-byte transaction hash                  | 블록파일넘버,파일내offset위치, 블록내offset위치                                                       | Transaction index 기록 (TxIndex On인 경우에만) |
|'c' + 32-byte transaction hash                  | Tx Version, Coinbase Tx 여부, Tx가 포함된 블록 높이, Tx 내 UTXO, UTXO의 scriptPubkey와 amount      | 트랜잭션 내 UTXO 데이터 조회용 (UTXO가 남은 경우)       |
|'B' -> 32-byte block hash                       | 가장 최신 Block Header Hash                                                               | 최신 Block이 있는지 확인 용                      |

## Mempool
LevelDB나 file data 형태로 관리됨
- 아직 블록에 포함되지 않은 Pending Transaction들을 저장 및 관리하는 방법
- 채굴자들은 Mempool 중에서 Transaction을 선택해서 신규 Block에 포함시킨다.
- Mempool 에 들어가고도 14일동안 처리 되지 않고 남아있는 Transaction은 Expired

## Bitcoin 검색의 한계
- 우리가 일반적으로 생각하는 검색의 조건은 단순검색, 범위 검색, 조건 검색등이 존재
- Bitcoin 은 Key-Value 기반 Database를 사용하고 있기 때문에, 검색에는 Key값으로만 검색이 가능
- 과거에는 Bitcoin-Qt, Paper wallet 등이 많이 사용되어 있는데, 이는 실제로 Transaction이 전송 중인지,
내가 전송을 잘 받았는지, 6-Confirm이 지났는지에 대한 확인이 어려움.
  - 이런 검색 측면에서의 문제점을 개선하고자 나온 것이 Explorer 사이트
  - 대표적으로 bitinfochart.com, blockchain.info 등이 존재

## 참고한 자료
- 한 번에 끝내는 블록체인 개발 A to Z
- https://github.com/bitcoin/bitcoin/blob/master/doc/files.md