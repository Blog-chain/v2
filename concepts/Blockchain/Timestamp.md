---
title: Timestamp
description: 블록체인에서 타임스탬프의 역할과 중요성을 다룹니다.
aliases:
  - blockchain timestamp
tags:
  - blockchain
date: 2024-08-10
---

## Summary

블록체인과 같은 분산 시스템에서 `timestamp`는 절대적인 시간으로서 동기화되기 어렵다. 따라서, 해시 연산으로 트랜잭션의 발생 순서만을 정의하는 상대적인 시간 개념을 사용한다. 그럼에도 여러 노드가 존재하면 다양한 이유로 문제가 발생할 수 있는데, 이 점을 [[Consensus]]가 보완한다.

## Description

timestamp의 역할은 어떤 이슈가 일어난 시간을 기록하여 사실 존재를 증명하는 전후 관계를 보장하는 것이다. 기존의 중앙집권적인 환경에서는 하나의 서버가 timestamp를 결정하였지만 P2P 네트워크에서는 모든 노드가 하나의 시간축을 공유하려면 절대적인 시간을 요구하는 것이 아니라 상대적 시간축을 저의하는 방법이 있다. 즉, 블록체인에서의 timestamp는 실제 거래가 발생한 정확한 시각이 아닌 거래의 순서를 저장하는 것이다. 예를 들어, 데이터 A, B
에 대하여 h(A) = B이면 B는 A 이후에 만들어졌음을 알 수 있다. 이런 재귀적인 방식으로 해시 함수를 사용하여 모든 데이터의 전후 관계를 파악할 수 있다.

## References

- [블록체인 타임스탬프 (blockchain timestamp)](https://steemit.com/kr/@brownbears/blockchain-timestamp)
- [타임스탬프의 역할과 중요성](https://bitcoinwiki.org/wiki/Trusted_timestamping)
- [Bitcoin 블록체인의 타임스탬프](https://www.investopedia.com/terms/b/block-bitcoin-block.asp)
- [타임스탬프와 블록체인의 투명성](https://www.investopedia.com/news/what-genesis-block-bitcoin-terms/)
- [timestamp 서버 이해하기](https://steemit.com/blockchain/@voidlink/2)
- [비트코인 블록 타임스탬프](https://en.bitcoin.it/wiki/Block_timestamp)

## Related Keywords

- [[Blockchain]]
- [[Block]]
- [[Node]]
- [[Merkle Tree]]
- [[Bitcoin]]
- [[Ethereum]]
