---
title: Blockchain
description: 블록체인 기술의 특징과 구성 요소
aliases:
  - distributed ledger
  - decentralized ledger
tags:
  - blockchain
date: 2024-08-10
---

## Summary

`Blockchain`은 탈중앙화를 지향하는 공개 분산 원장 시스템입니다. 트랜잭션 데이터는 [[Block]] 단위로 기록되고, 이들이 P2P 방식의 체인 형태로 연결되어 있는 구조를 가지고 있습니다.

> P2P(Peer-to-Peer) : 상호 연결된 노드(peer)들이 서로 간에 자원을 공유하는 네트워크

## Description

## 블록체인의 특징

- immutable(불변성)
  - 네트워크 참여자 간 합의 이후 체인에 기록된 데이터는 수정 또는 삭제하지 않음
  - 그러나 커뮤니티의 합의에 따라 수정될 수 있다.
- transparent(투명성)
  - 네트워크 참여자는 체인에 기록된 데이터를 누구나 열람할 수 있음
- distributed(분산성) / decentralized(탈중앙성)
  - 중앙 관리자나 운영자 없이 네트워크 참여자들끼리 실시간으로 정보 공유 및 책임을 분담함
  - 이상이 생기면 운영자가 없기 때문에 네트워크 참여자 모두의 책임이 된다.
- secure(보안성)
  - 암호학적 장치로 인해 체인의 내용을 임의로 위조 또는 변조 불가능
- 기타
  - 합의 알고리즘의 비효율성(PoW)
  - EOA 키 관리의 어려움
  - 제한적인 저장 공간
  - privacy / traceability
  - ‘FLP Impossibility’ 이슈로 모든 구현체는 Safety(결정론적 동작; 합의 결과 불변) 또는 Liveness(비동기적 동작; 발생된 모든 상태 변화의 유한 시간 내 합의) 중 일부를 희생한 구조를 갖는다.

## 블록체인의 구성 요소

1. **블록 (Block)**: 트랜잭션 데이터를 포함하는 데이터 구조입니다. 각 블록은 이전 블록의 해시 값을 포함하여 체인 형태로 연결됩니다.
2. **노드 (Node)**: 블록체인 네트워크를 구성하는 개별 컴퓨터로, 모든 트랜잭션을 검증하고 블록을 생성 및 전파하는 역할을 합니다.
3. **합의 알고리즘 (Consensus)**: 블록체인 네트워크의 모든 노드가 동일한 원장을 유지하도록 보장하는 메커니즘입니다. 예를 들어, PoW(작업 증명), PoS(지분 증명), DPoS(위임 지분 증명) 등이 있습니다.
4. **스마트 계약(Smart Contract; Optional)**: 블록체인 상에서 자동으로 실행되는 계약입니다. 특정 조건이 충족되면 계약이 자동으로 실행되며, 이는 신뢰성과 투명성을 높입니다

## References

- https://ethereum.org/en/glossary/#blockchain
- https://ko.wikipedia.org/wiki/블록체인
- [Understanding Blockchain Technology](https://builtin.com/blockchain)
- https://bitcoinwiki.org/wiki/blockchain

## Related Keywords

- [[Block]]
- [[Timestamp]]
- [[Node]]
- [[Consensus]]
- [[Archive Node]]
- [[Merkle Tree]]
- [[Smart Contract]]
- [[Bitcoin]]
- [[Ethereum]]
- [[Mainnet]]
- [[Testnet]]
