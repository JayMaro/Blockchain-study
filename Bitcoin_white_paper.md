# Bitcoin white paper 분석

## Abstract:
A purely peer-to-peer version of electronic cash would allow online
payments to be sent directly from one party to another without going through a
financial institution

: 개인 대 개인의 온라인 결제를 금융기관을 거치지 않고 이뤄질 것이다.

The network timestamps transactions by hashing them into an ongoing chain of
hash-based proof-of-work, forming a record that cannot be changed without redoing
the proof-of-work

: 네트워크는 거래를 해싱해 타임스탬프를 찍어 해시 기반 작업증명을 연결할 사슬로 만든다.
-> 블록 체인

nodes can leave and rejoin the network at will, accepting the longest
proof-of-work chain as proof of what happened while they were gone.

: 노드는 네트워크를 떠났다 올 수 있다. 가장 긴 작업 증명 체인을 가져오면 그동안 있었던 일들을 모두 알 수 있기 때문이다.


## Introduction
What is needed is an electronic payment system based on cryptographic proof instead of trust,
allowing any two willing parties to transact directly with each other without the need for a trusted
third party

: 필요한 것은 신뢰 대신 암호학적 증명에 기반하고 신뢰받는 제 3자를 필요로 하지 않는 전자 결제 시스템이다.

## Transactions
