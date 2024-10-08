---
title: Validator
description: 블록체인 네트워크에서 검증자(Validator)의 개념, 역할, 및 중요성을 다룹니다.
aliases:
  - blockchain validator
  - consensus validator
tags:
  - blockchain
  - node
  - consensus
  - ethereum
date: 2024-07-22
---

## Summary

`Validator`는 블록체인 네트워크에서 트랜잭션을 검증하고 새로운 블록을 생성하는 역할을 하는 참여자입니다.

## Description

`Validator`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 트랜잭션을 검증하고 새로운 블록을 생성하는 참여자로, 네트워크의 무결성과 보안을 유지하는 데 핵심적인 역할을 합니다. Validator의 주요 특징은 다음과 같습니다:

1. **트랜잭션 검증**: Validator는 네트워크에서 발생하는 트랜잭션을 검증하여, 트랜잭션의 무결성을 보장합니다.
2. **블록 생성**: Validator는 검증된 트랜잭션을 모아 새로운 블록을 생성합니다. 이는 네트워크의 지속적인 운영을 보장하는 데 중요한 역할을 합니다.
3. **보상**: Validator는 검증 작업에 대한 보상을 받습니다. 이는 Validator가 네트워크에 계속 참여하도록 유도하는 인센티브 역할을 합니다.

## How this works

validator가 되기 위해, user는 32ETH를 컨트랙트에 예치하고, execution client, consensus client, validator client이 세 개의 software를 실행시켜야 한다.

ETH를 deposit하면, user 는 새로운 validator가 네트워크에 들어오는 것을 제한하는 activation queue에 들어가게 된다. activate되면, validator는 다른 validator node로부터 새로운 블록을 받고(propose받음), propose된 Ethereum의 state 변화가 valid한지 검사해보기 위해 트랜잭션들을 다시 검증하고, 블록에 서명된 서명을 검증하게 된다.

> 원문: The transactions delivered in the block are re-executed to check that the proposed changes to Ethereum's state are valid, and the block signature is checked.

그 다음, validator는 네트워크를 통해 블록에 대한 favor를 나타내는 attestation message(투표)를 보내게 된다.

## Ethereum의 검증 과정

Ethereum에서의 검증 과정은 다음과 같은 단계로 이루어집니다:

1. **트랜잭션 전송**: 사용자가 Ethereum 네트워크에 트랜잭션을 전송하면, 해당 트랜잭션은 네트워크의 각 노드에 전파됩니다.
2. **트랜잭션 수신 및 검증**: Validator 노드는 수신한 트랜잭션을 검증합니다. 검증 과정에서 트랜잭션의 서명이 올바른지, 발신자의 잔액이 충분한지 등을 확인합니다.
3. **트랜잭션 저장**: 검증된 트랜잭션은 Mempool에 저장됩니다. Mempool은 아직 블록에 포함되지 않은 대기 중인 트랜잭션을 저장하는 공간입니다.
4. **블록 제안**: 검증자는 Mempool에서 트랜잭션을 선택하여 새로운 블록을 제안합니다. 블록 제안 과정에서 트랜잭션의 우선 순위는 보통 수수료에 따라 결정됩니다.
5. **블록 검증 및 추가**: 제안된 블록은 다른 검증자들에 의해 검증됩니다. 검증이 완료되면 블록은 블록체인에 추가됩니다. 이 과정에서 검증자들은 블록의 무결성과 유효성을 확인합니다.
6. **보상 수령**: 블록을 성공적으로 제안하고 검증한 검증자는 보상을 받습니다. 보상은 보통 스테이킹된 암호화폐의 양과 트랜잭션 수수료로 이루어집니다.

## 구성

### LMD-GHOST 기반의 체인 연결

Ethereum의 검증자는 LMD-GHOST(Latest Message Driven - Greedy Heaviest Observed SubTree) 알고리즘을 기반으로 체인에 연결합니다. 이 알고리즘은 검증자가 최신 메시지를 기반으로 가장 무거운 서브트리를 선택하여 포크 선택 규칙을 결정합니다. 이는 네트워크의 안정성과 보안을 강화하는 데 중요한 역할을 합니다.

### Ethereum 기반의 주요 기술

Ethereum의 검증자는 다양한 기술을 활용하여 네트워크의 보안을 유지하고 합의를 달성합니다. 그 중 중요한 기술은 다음과 같습니다:

- **Validator Node**: 검증자가 실행하는 노드로, 블록을 생성하고 트랜잭션을 검증하는 역할을 합니다.
- **Slashing**: 악의적인 행동을 방지하기 위해, 잘못된 행동을 한 검증자의 스테이킹된 자산의 일부를 몰수하는 방식입니다.
- **Casper**: Ethereum의 PoS 구현 중 하나로, 검증자가 블록을 생성하고 합의에 도달하는 과정을 정의합니다.

## References

- [Validator 설명](https://en.wikipedia.org/wiki/Proof_of_stake#Validator)

## Related Keywords

- [[Staking]]
- [[PoS]]
- [[Ethereum]]
- [[Validator]]
- [[Slashing]]
- [[Casper]]
- [[LMD-GHOST]]
