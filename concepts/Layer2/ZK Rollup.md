---
title: ZK Rollup
description: 블록체인 네트워크에서 ZK 롤업(ZK Rollup)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [zk rollup, zero-knowledge rollup, blockchain zk rollup]
tags: [technology, blockchain, zk rollup, layer2]
date: 2024-07-22
---

## ZK Rollup

### Summary

`ZK Rollup`는 블록체인 네트워크에서 영지식 증명(Zero-Knowledge Proof)을 사용하여 트랜잭션을 롤업하고, 이를 메인 체인에 제출하는 Layer2 솔루션입니다.

### Description

`ZK Rollup`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 영지식 증명(Zero-Knowledge Proof)을 사용하여 트랜잭션을 롤업하고, 이를 메인 체인에 제출하는 Layer2 솔루션입니다. ZK Rollup의 주요 특징은 다음과 같습니다:

1. **확장성 향상**: ZK Rollup은 메인 체인의 트랜잭션 처리 부담을 줄여, 네트워크의 확장성을 향상시킵니다.
2. **보안 강화**: 영지식 증명을 사용하여 트랜잭션의 유효성을 검증하므로 보안이 강화됩니다.
3. **트랜잭션 데이터 저장**: 트랜잭션 데이터는 오프체인에서 관리되어 메인 체인의 데이터 부담을 줄입니다.

ZK Rollup 체인은 오프체인 프로토콜로, 이더리움 블록체인 위에서 운영되며, 온체인 이더리움 스마트 계약에 의해 관리됩니다. ZK Rollups는 트랜잭션을 메인넷 외부에서 실행하지만, 주기적으로 오프체인 트랜잭션 배치를 온체인 롤업 계약에 커밋합니다.

### ZK Rollup의 주요 기술

ZK Rollup 솔루션은 다음과 같은 특징을 가집니다:

1. **트랜잭션 롤업**: 트랜잭션을 묶어서 오프체인에서 실행하고, 이를 메인 체인에 제출합니다.
2. **영지식 증명**: 영지식 증명을 사용하여 트랜잭션의 유효성을 검증합니다. 자세한 내용은 [여기](https://ethereum.org/en/developers/docs/scaling/zk-rollups/#what-are-zk-rollups)에서 확인할 수 있습니다.

### ZK Rollup과 Optimistic Rollup 비교

ZK Rollup과 Optimistic Rollup은 모두 Layer2 솔루션으로, 메인 체인의 확장성을 향상시키기 위해 사용됩니다. 하지만 두 솔루션은 몇 가지 중요한 차이점이 있습니다:

1. **보안 방식**:

   - **ZK Rollup**: 영지식 증명을 사용하여 트랜잭션의 유효성을 검증합니다. 이는 높은 보안을 제공합니다.
   - **Optimistic Rollup**: 트랜잭션의 유효성을 검증하기 위해 fraud proof를 사용합니다. 이는 일정 기간 동안 검증자가 트랜잭션의 유효성에 대해 이의를 제기할 수 있도록 합니다.

2. **확정성**:

   - **ZK Rollup**: 트랜잭션이 즉시 확정되며, 추가적인 검증이 필요하지 않습니다.
   - **Optimistic Rollup**: 트랜잭션이 확정되기까지 일정 기간(일반적으로 1주일)의 챌린지 기간이 필요합니다.

3. **처리 속도**:
   - **ZK Rollup**: 영지식 증명을 생성하는 데 시간이 걸리므로, 초기 설정이 복잡할 수 있습니다.
   - **Optimistic Rollup**: 트랜잭션의 검증이 지연될 수 있지만, 초기 설정이 상대적으로 간단합니다.

### References

- [ZK Rollup 설명](https://en.wikipedia.org/wiki/ZK-Rollup)
- [ZK Rollup의 작동 원리](https://ethereum.org/en/developers/docs/scaling/zk-rollups/#what-are-zk-rollups)
- [Gemini의 ZK Rollup 설명](https://www.gemini.com/cryptopedia/search?query=zk-rollup)
- [Optimistic Rollup 설명](https://docs.optimism.io/stack/protocol/rollup/overview)
- [Optimistic Rollup의 작동 원리](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/)

### Related Keywords

- [[Rollup]]
- [[Layer2]]
- [[Blockchain]]
- [[Scalability]]
- [[Security]]
