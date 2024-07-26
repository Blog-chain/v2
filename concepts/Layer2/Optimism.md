---
title: Optimism
description: 블록체인 네트워크에서 옵티미즘(Optimism)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [optimism, blockchain optimism, layer2 optimism]
tags: [technology, blockchain, optimism, layer2]
date: 2024-07-22
---

## Optimism

### Summary

`Optimism`은 블록체인 네트워크에서 트랜잭션을 오프체인에서 처리하고, 이를 메인 체인에 제출하여 확장성을 향상시키는 Layer2 솔루션입니다.

### Description

`Optimism`은 블록체인 네트워크에서 중요한 역할을 합니다. 이는 트랜잭션을 오프체인에서 처리하고, 이를 메인 체인에 제출하여 네트워크의 확장성을 향상시키는 Layer2 솔루션입니다. Optimism의 주요 특징은 다음과 같습니다:

1. **확장성 향상**: Optimism은 메인 체인의 트랜잭션 처리 부담을 줄여, 네트워크의 확장성을 향상시킵니다.
2. **보안 강화**: Optimism은 트랜잭션 데이터를 오프체인에서 처리하여, 메인 체인의 보안을 강화합니다.
3. **트랜잭션 데이터 저장**: 트랜잭션 데이터는 오프체인에서 관리되어 메인 체인의 데이터 부담을 줄입니다.

Optimism은 이더리움 개발자들에 의해 구축된 L2 블록체인으로, 트랜잭션을 배치 처리하여 메인 체인에 제출합니다. 사용자들은 저렴하고 거의 즉각적인 트랜잭션을 이용할 수 있으며, 개발자들은 이더리움 앱을 위한 빠르고 안정적이며 확장 가능하고 안전한 솔루션으로 Optimism을 사용할 수 있습니다.

### Optimism의 주요 기술

Optimism 솔루션은 다음과 같은 특징을 가집니다:

1. **Optimistic Rollup**: Optimism은 Optimistic Rollup 기술을 사용하여 트랜잭션을 처리합니다. 이는 트랜잭션을 배치하여 오프체인에서 처리하고, 메인 체인에 주기적으로 결과를 제출합니다.
2. **Fraud Proofs**: 트랜잭션의 유효성을 보장하기 위해 검증자(Validator)가 이의 제기를 통해 트랜잭션을 검증합니다. 이의 제기와 검증 과정은 메인 체인에서 수행되며, 이는 네트워크의 보안을 유지하는 데 중요한 역할을 합니다.
3. **Fast Finality**: Optimism은 빠른 트랜잭션 확정을 제공합니다. 사용자는 거의 즉각적인 트랜잭션을 경험할 수 있으며, 이는 실시간 애플리케이션에 적합합니다.

### Optimism의 구현

Optimism의 구현체는 다음과 같습니다:

1. **Optimistic Rollup 구현체**: Optimism은 Optimistic Rollup 기술을 사용하여 구현됩니다. 이는 이더리움 메인 체인과 상호작용하며, 트랜잭션을 오프체인에서 처리합니다.
2. **Optimism Gateway**: 사용자는 Optimism Gateway를 통해 메인 체인과 Optimism 간에 자산을 전송할 수 있습니다. 이는 이더리움 네트워크와의 상호 운용성을 제공합니다.
3. **Optimism SDK**: 개발자는 Optimism SDK를 사용하여 이더리움 애플리케이션을 Optimism 네트워크에서 실행할 수 있습니다. 이는 이더리움의 스마트 계약과 호환됩니다.

### 이의 제기와 Fraud Proof

Optimism에서 이의 제기는 검증자가 트랜잭션의 유효성에 대해 이의를 제기하는 과정을 말합니다. 이의 제기 과정은 다음과 같은 특징을 가집니다:

- **Validator의 이의 제기**: 검증자는 트랜잭션의 유효성에 대해 이의를 제기할 수 있습니다. 이의 제기를 통해 트랜잭션의 유효성을 검증하며, 이는 네트워크의 보안을 유지하는 데 중요한 역할을 합니다.
- **Fraud Proofs**: 이의 제기는 fraud proof를 통해 검증됩니다. fraud proof가 명백히 악의적인 목적으로 제기되면, 거버넌스나 security council 등 상위 권한의 시스템에 의해 밸리데이터에서 추방되거나 슬래싱됩니다. 자세한 내용은 [여기](https://docs.optimism.io/stack/protocol/rollup/overview)에서 확인할 수 있습니다.
- **챌린지 프로세스**: 대부분의 Optimistic Rollup에서 챌린지는 permissioned entity (예: 밸리데이터)만 제기할 수 있도록 구현되어 있습니다. 챌린지 프로세스는 여러 트랜잭션에 나눠서 진행되며, 담보는 미리 예치해야 합니다. 자세한 내용은 [여기](https://blog.kroma.network/about-the-first-successful-challenge-on-kroma-mainnet-aeca715b05d7)에서 확인할 수 있습니다.

### References

- [Optimism 설명](<https://en.wikipedia.org/wiki/Optimism_(blockchain)>)
- [Optimism의 작동 원리](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/)
- [Gemini의 Optimism 설명](https://www.gemini.com/cryptopedia/search?query=optimism)
- [Optimism 개발 문서](https://docs.optimism.io/stack/protocol/rollup/overview)
- [Optimistic Rollup 설명](https://docs.optimism.io/stack/protocol/rollup/overview)
- [Optimistic Rollup의 작동 원리](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/)
- [Kroma 체인의 첫 성공적인 챌린지 사례](https://blog.kroma.network/about-the-first-successful-challenge-on-kroma-mainnet-aeca715b05d7)
- [Challenge Periods에 대한 설명](https://kelvinfichter.com/pages/thoughts/challenge-periods/)

### Related Keywords

- [[Rollup]]
- [[Layer2]]
- [[Blockchain]]
- [[Scalability]]
- [[Security]]
