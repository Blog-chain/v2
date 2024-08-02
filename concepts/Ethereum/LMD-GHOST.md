---
title: LMD-GHOST Algorithm
description: An overview of the LMD-GHOST consensus algorithm used in Ethereum.
aliases:
  - Latest Message Driven Greediest Heaviest Observed SubTree
tags:
  - ethereum
  - consensus
date: 2024-08-02
---

# Summary

LMD-GHOST (Latest Message Driven Greediest Heaviest Observed SubTree) 알고리즘은 이더리움 2.0에서 사용되는 합의 알고리즘입니다. 이 알고리즘은 블록체인의 헤드를 결정하는 데 사용되며, 네트워크 참여자들의 최신 메시지를 기반으로 가장 무거운 서브트리를 선택합니다.

### Description

LMD-GHOST 알고리즘은 다음과 같은 방식으로 작동합니다:

1. **블록 추가**: 새로운 블록이 생성되면, 해당 블록은 부모 블록의 자식으로 추가됩니다.
2. **투표**: 네트워크 참여자들은 자신이 선호하는 블록에 대해 투표를 합니다. 이 투표는 블록의 `votes` 속성에 반영됩니다.
3. **헤드 결정**: LMD-GHOST 알고리즘은 제네시스 블록에서 시작하여 가장 많은 투표를 받은 자식 블록을 선택합니다. 이 과정을 반복하여 체인의 헤드를 결정합니다.

이 알고리즘은 블록의 깊이와 상관없이 각 블록에 대한 투표 수를 기반으로 체인의 헤드를 결정합니다. 이를 통해 네트워크의 최신 상태를 반영하고, 가장 무거운 서브트리를 선택할 수 있습니다.

### References

- https://eth2book.info/capella/part2/consensus/lmd_ghost

### Related Keywords

- [[Ethereum]]
