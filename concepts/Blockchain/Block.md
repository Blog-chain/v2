---
title: Block
description: 블록체인에서 블록의 개념과 구조의 일부를 소개합니다.
aliases:
  - 블록
tags:
  - blockchain
date: 2024-08-12
---

## Summary

블록은 트랜잭션의 기록입니다. 블록체인에서 블록은 이전 블록의 해시값을 포함하고 있으며, 이를 통해 체인의 무결성을 보장합니다. 블록은 트랜잭션 데이터와 블록 헤더로 구성되어 있으며, 각 블록은 일정한 주기로 생성됩니다. 블록은 블록체인 네트워크의 합의 알고리즘에 따라 생성되며, 블록 생성자는 보상을 받습니다. 블록은 블록체인의 핵심 구성 요소이며, 블록체인의 모든 참여자가 동일한 블록체인 상태를 유지할 수 있도록 합니다.

블록의 정확한 구조는 체인마다 다릅니다. 헤더와 바디의 속성 종류가 다르고, 같은 명칭이라도 다른 의미로 사용될 수 있습니다. 예를 들어, 비트코인에서 [[nonce]]는 PoW 합의 방식에서 [[mining]]을 위한 변수입니다. 반면, 이더리움에서는 리플레이 공격을 방지하는 각 계정의 트랜잭션 횟수 카운터 변수입니다.

## Description

이더리움 기반의 블록 정보는 설명이 방대하므로 직접 링크를 참조하는 것을 추천합니다.
![ethbook-block](ethbook-block.png)
보다 자세한 내용은 [eth2book](https://eth2book.info/capella/annotated-spec/)을 참고하세요.

## References

- [eth2book / One Page Annotated Spec](https://eth2book.info/capella/annotated-spec/)
- https://ethereum.org/en/developers/docs/blocks/
- [Ethereum Blocks](https://ethereum.org/en/developers/docs/blocks/)
- [Execution data](https://ethereum.org/en/developers/docs/data-and-analytics/block-explorers/#execution-data)
- [블록체인-구조와-블록체인-트랜잭션](https://www.codestates.com/blog/content/블록체인-구조와-블록체인-트랜잭션)
- [Bitcoin Block](https://bitcoinwiki.org/wiki/block)
- [블록의 구조](https://m.upbitcare.com/academy/education/blockchain/64)
- [What Is a Block in the Crypto Blockchain, and How Does It Work?](https://www.investopedia.com/terms/b/block-bitcoin-block.asp)
- [What Is the Genesis Block in Bitcoin Terms?](https://www.investopedia.com/news/what-genesis-block-bitcoin-terms/)

## Related Keywords

- [[Blockchain]]
- [[Timestamp]]
- [[Node]]
- [[Merkle Tree]]
- [[Bitcoin]]
- [[Ethereum]]
