---
title: Bitcoin
description: 비트코인의 개념과 특징
aliases:
  - BTC
  - 비트코인
tags:
  - blockchain
  - cryptocurrency
  - bitcoin
date: 2024-08-12
---

## Summary

`Bitcoin`은 2008년 익명의 프로그래머 Satoshi Nakamoto에 의해 발명된 최초의 분산형 디지털 암호화폐입니다. 이는 탈중앙화된 블록체인 기술을 기반으로 하며, 작업 증명(Proof of Work) 합의 메커니즘을 사용합니다. P2P 방식으로 설계되어 있으며 해시 함수로는 Double SHA-256를 사용합니다. 채굴 보상이 절반으로 줄어드는 반감기가 4년으로 설정되어 총 발행량이 2100만 BTC로 제한됩니다.

## Description

Bitcoin의 주요 기술적 특징은 다음과 같습니다:

1. **블록체인 (Blockchain)**: Bitcoin은 트랜잭션을 기록하는 분산 원장 시스템을 사용합니다. 각 블록은 이전 블록의 해시를 포함하며, 이는 체인 형태로 연결됩니다.
2. **작업 증명 (Proof of Work, PoW)**: 비트코인은 네트워크의 보안을 유지하고 새로운 블록을 생성하기 위해 작업 증명 합의 메커니즘을 사용합니다. 채굴자들은 복잡한 수학 문제를 해결하여 블록을 생성하고, 이에 대한 보상을 받습니다.
3. **이중 지불 방지 (Double-Spending Prevention)**: 비트코인은 각 코인이 고유한 식별자를 가지는 **UTXO (Unspent Transaction Output)** 모델을 사용하여 이중 지불을 방지합니다. UTXO는 소비되지 않은 트랜잭션 출력을 의미하며, 각 UTXO는 한 번만 사용될 수 있습니다. 블록체인에 모든 트랜잭션 기록이 투명하게 저장되고, PoW를 통해 블록의 위변조를 방지하여 이중 지불 시도를 차단합니다. 즉, 비트코인은 은행 계좌처럼 잔액을 기록하는 것이 아니라, 각 코인이 개별적으로 관리되는 방식으로 작동합니다.

## References

- [위키백과 / 비트코인](https://ko.wikipedia.org/wiki/%EB%B9%84%ED%8A%B8%EC%BD%94%EC%9D%B8)
- [비트코인은 무엇인가?](https://m.upbitcare.com/academy/education/coin/36)
- [비트코인의 블록 개념](https://bitcoinwiki.org/wiki/block)
- [A Gentle Introduction to Blockchain Technology](https://bitsonblocks.net/2015/09/09/gentle-introduction-blockchain-technology/)

## Related Keywords

- [[Blockchain]]
- [[Cryptocurrency]]
- [[Proof of Work]]
- [[Mining]]
- [[UTXO]]
