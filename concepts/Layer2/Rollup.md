---
title: Rollup
description: 블록체인 네트워크에서 롤업(Rollup)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [rollup, blockchain rollup, layer2 rollup]
tags: [technology, blockchain, rollup, layer2]
date: 2024-07-22
---

## Rollup

### Summary

`Rollup`는 블록체인 네트워크에서 트랜잭션을 묶어 메인 체인에 제출하여 확장성을 향상시키는 Layer2 솔루션입니다.

### Description

`Rollup`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 트랜잭션을 묶어 메인 체인에 제출하여 네트워크의 확장성을 향상시키는 Layer2 솔루션입니다. Rollup의 주요 특징은 다음과 같습니다:

1. **트랜잭션 묶음**: Rollup은 여러 트랜잭션을 묶어 하나의 트랜잭션으로 메인 체인에 제출합니다. 이는 메인 체인의 트랜잭션 처리 부담을 줄입니다.
2. **저렴한 비용**: Rollup은 트랜잭션 수수료를 줄여, 사용자에게 더 저렴한 비용을 제공합니다.
3. **빠른 처리 속도**: Rollup은 트랜잭션 처리 속도를 높여, 사용자에게 더 빠른 서비스를 제공합니다.

Rollup 솔루션은 트랜잭션을 묶어서(또는 '롤업') 오프체인에서 실행합니다.

### Rollup의 주요 기술

Rollup 솔루션에는 여러 가지 기술이 사용됩니다. 그 중 중요한 몇 가지를 소개합니다:

1. **Optimistic Rollup**

   - 레이어 2(L2) 프로토콜로, 이더리움의 기본 레이어의 처리량을 확장하기 위해 설계되었습니다.
   - Optimistic Rollup은 트랜잭션 결과를 온체인에 게시하여 메인넷으로부터 보안을 확보합니다. 트랜잭션 데이터는 다른 곳에 저장됩니다.
   - 자세한 내용은 [여기](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/)에서 확인할 수 있습니다.

2. **ZK Rollup**
   - 트랜잭션을 묶어서 오프체인에서 실행합니다.
   - ZK-rollup 체인은 이더리움 블록체인 위에서 운영되는 오프체인 프로토콜로, 온체인 이더리움 스마트 계약에 의해 관리됩니다. ZK-rollups는 트랜잭션을 메인넷 외부에서 실행하지만, 주기적으로 오프체인 트랜잭션 배치를 온체인 롤업 계약에 커밋합니다.
   - 자세한 내용은 [여기](https://ethereum.org/en/developers/docs/scaling/zk-rollups/#what-are-zk-rollups)에서 확인할 수 있습니다.

### References

- [Rollup 설명](https://www.techopedia.com/definition/rollup)
- [Rollup의 작동 원리](https://ethereum.org/en/glossary/#rollup)
- [Gemini의 Rollup 설명](https://www.gemini.com/cryptopedia/search?query=rollup)

### Related Keywords

- [[Layer2]]
- [[Blockchain]]
- [[Scalability]]
- [[Optimistic Rollup]]
- [[ZK Rollup]]
