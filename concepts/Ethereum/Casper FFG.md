---
title: Casper FFG Algorithm
description: An overview of the Casper FFG consensus algorithm used in Ethereum.
aliases:
  - Friendly Finality Gadget
tags:
  - ethereum
  - consensus
date: 2024-08-02
---

# Summary

Casper FFG (Friendly Finality Gadget)는 이더리움 2.0에서 사용되는 합의 알고리즘입니다. 이 알고리즘은 PoW(Proof of Work)와 PoS(Proof of Stake)를 결합하여 블록체인의 최종성을 보장합니다.

### Description

Casper FFG는 다음과 같은 방식으로 작동합니다:

1. **투표**: 검증자들은 특정 체크포인트 블록에 대해 투표를 합니다. 이 투표는 블록의 최종성을 보장하기 위해 사용됩니다.
2. **체크포인트**: 블록체인은 일정 간격으로 체크포인트를 설정합니다. 각 체크포인트는 이전 체크포인트와 연결됩니다.
3. **최종성**: 특정 수의 검증자들이 체크포인트 블록에 대해 투표하면, 해당 블록은 최종성을 갖게 됩니다. 이는 블록이 변경될 수 없음을 의미합니다.

Casper FFG는 PoW와 PoS의 장점을 결합하여, 네트워크의 보안성과 효율성을 높입니다. PoW는 블록을 생성하는 데 사용되고, PoS는 블록의 최종성을 보장하는 데 사용됩니다.

### References

- https://eth2book.info/capella/part2/consensus/casper_ffg/

### Related Keywords

- [[Consensus]]
