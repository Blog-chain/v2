---
title: Slashing
description: 블록체인 네트워크에서 슬래싱(Slashing)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [slashing, blockchain slashing, validator slashing]
tags: [technology, blockchain, slashing, consensus]
date: 2024-07-22
---

## Slashing

### Summary

`Slashing`은 블록체인 네트워크에서 검증자가 규칙을 위반할 경우 일정량의 스테이킹된 자산을 몰수하는 메커니즘입니다.

### Description

`Slashing`은 블록체인 네트워크에서 검증자의 악의적인 행동이나 규칙 위반을 방지하기 위해 사용되는 중요한 메커니즘입니다. 검증자가 잘못된 행동을 할 경우, 네트워크는 검증자의 스테이킹된 자산의 일부를 몰수하여 경제적 처벌을 가합니다. Slashing의 주요 특징은 다음과 같습니다:

1. **보안 강화**: Slashing은 검증자가 네트워크 규칙을 준수하도록 강제하여 보안을 강화합니다. 이를 통해 네트워크의 무결성을 유지합니다.
2. **경제적 처벌**: 잘못된 행동을 한 검증자는 Slashing을 통해 경제적 손실을 입습니다. 이는 검증자가 규칙을 준수하도록 유도하는 중요한 인센티브 역할을 합니다.
3. **네트워크 무결성 유지**: Slashing 메커니즘은 검증자의 잘못된 행동을 방지하여 네트워크의 무결성과 신뢰성을 유지합니다.

### Ethereum 기반의 Slashing

Ethereum에서는 Slashing이 중요한 역할을 합니다. 특히, 검증자의 악의적인 행동을 방지하고 네트워크의 보안을 유지하기 위해 다양한 Slashing 메커니즘이 사용됩니다.

- **이중 서명 방지**: 검증자가 동일한 블록 높이에서 두 개 이상의 블록을 서명하는 경우, Slashing이 적용됩니다.
- **검증 실패 방지**: 검증자가 검증 작업을 수행하지 않거나, 잘못된 검증을 수행하는 경우에도 Slashing이 적용됩니다.

### References

- [Slashing 설명](https://ethereum.org/en/glossary/#validator-lifecycle)

### Related Keywords

- [[Validator]]
- [[Proof of Stake]]
- [[Blockchain]]
- [[Security]]
- [[Validator Node]]
