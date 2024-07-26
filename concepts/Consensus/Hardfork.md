---
title: Hard Fork
description: 블록체인 네트워크에서 하드 포크(Hard Fork)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [hard fork, blockchain hard fork]
tags: [technology, blockchain, hard fork, consensus]
date: 2024-07-22
---

## Hard Fork

### Summary

`Hard Fork`는 블록체인 네트워크에서 기존 블록체인과 호환되지 않는 새로운 규칙으로 업데이트되는 과정을 의미합니다.

### Description

`Hard Fork`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 기존 블록체인과 호환되지 않는 새로운 규칙으로 업데이트되는 과정을 의미하며, 네트워크의 주요 변화나 업그레이드를 반영합니다. Hard Fork의 주요 특징은 다음과 같습니다:

1. **불가역적 변경**: Hard Fork는 네트워크의 규칙을 영구적으로 변경합니다. 일단 실행되면, 새로운 규칙을 따르지 않는 노드는 네트워크에 참여할 수 없게 됩니다.
2. **네트워크 분리**: Hard Fork는 기존 네트워크를 두 개의 별도의 체인으로 분리시킵니다. 이는 새로운 통화의 생성을 의미할 수도 있습니다.
3. **커뮤니티 합의 필요**: 대부분의 경우, Hard Fork는 네트워크 참여자들 사이의 합의를 필요로 합니다. 그러나 합의에 도달하지 못하면 분열이 발생할 수 있습니다.

### Hard Fork의 예시

- **비트코인과 비트코인 캐시**: 2017년, 비트코인 네트워크는 블록 크기를 늘리기 위한 Hard Fork를 경험했습니다. 이로 인해 비트코인 캐시라는 새로운 암호화폐가 생성되었습니다.
- **이더리움과 이더리움 클래식**: 2016년, DAO 공격 후 이더리움 네트워크는 투표를 통해 자금을 돌려받기 위한 Hard Fork를 실행했습니다. 이로 인해 이더리움 클래식이라는 새로운 체인이 생겨났습니다.
- **이더리움의 이스탄불 업그레이드**: EVM opcode를 추가하는 이 업그레이드는 반드시 호환성을 보장할 수 없게 되어 Hard Fork로 진행되었습니다.

  - EIP-1884: 특정 EVM 오퍼레이션의 가스 비용 재조정
  - EIP-2028: 데이터 전송 가스 비용 감소
  - EIP-1344: 체인 ID 오퍼레이션 코드 추가

### Tips

- Ethereum의 The Merge는 기존 PoW 방식의 합의를 PoS로 전환한 Hard Fork입니다. 기존 방식을 유지하고 있는 체인은 [`ETHW`](https://coinmarketcap.com/currencies/ethereum-pow)로 남아있습니다.

### References

- [Hard Fork 설명](https://en.wikipedia.org/wiki/Hard-Fork)
- [Hard Fork의 작동 원리](https://ethereum.org/en/glossary/#hard-fork)

### Related Keywords

- [[Consensus]]
- [[Blockchain]]
- [[Security]]
- [[Upgrade]]
- [[Ethereum 2.0]]
- [[Constantinople]]
