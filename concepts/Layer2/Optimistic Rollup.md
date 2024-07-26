---
title: Optimistic Rollup
description: 블록체인 네트워크에서 옵티미스틱 롤업(Optimistic Rollup)의 개념, 역할, 및 중요성을 다룹니다.
aliases:
  [optimistic rollup, blockchain optimistic rollup, layer2 optimistic rollup]
tags: [technology, blockchain, optimistic rollup, layer2]
date: 2024-07-22
---

## Optimistic Rollup

### Summary

`Optimistic Rollup`는 블록체인 네트워크에서 트랜잭션을 롤업하여 메인 체인에 제출하고, 검증자가 트랜잭션의 유효성을 검증하는 Layer2 솔루션입니다.

### Description

`Optimistic Rollup`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 트랜잭션을 롤업하여 메인 체인에 제출하고, 검증자가 트랜잭션의 유효성을 검증하는 Layer2 솔루션입니다. Optimistic Rollup의 주요 특징은 다음과 같습니다:

1. **확장성 향상**: Optimistic Rollup은 메인 체인의 트랜잭션 처리 부담을 줄여, 네트워크의 확장성을 향상시킵니다.
2. **보안 확보**: Optimistic Rollup은 메인넷에 트랜잭션 결과를 게시하여 보안을 확보합니다.
3. **트랜잭션 데이터 저장**: 트랜잭션 데이터는 다른 곳에 저장되어 메인 체인의 데이터 부담을 줄입니다.

Optimistic Rollup은 이더리움 개발자들에 의해 구축된 L2 블록체인으로, 트랜잭션을 배치 처리하여 메인 체인에 제출합니다. 사용자들은 저렴하고 거의 즉각적인 트랜잭션을 이용할 수 있으며, 개발자들은 이더리움 앱을 위한 빠르고 안정적이며 확장 가능하고 안전한 솔루션으로 Optimism을 사용할 수 있습니다.

### Optimistic Rollup의 주요 기술

Optimistic Rollup 솔루션은 이더리움의 기본 레이어의 처리량을 확장하기 위해 설계되었습니다. Optimistic Rollup의 구현은 fraud proof를 기반으로 하며, 다음과 같은 특징을 가집니다:

- **Validator의 이의 제기와 fraud proof**: 검증자는 트랜잭션의 유효성을 확인하기 위해 이의 제기를 할 수 있으며, 이의 제기는 fraud proof를 통해 검증됩니다. 이의 제기를 계속적으로 발생시킬 경우 L2의 최신 체크포인트가 확정되지 않으므로 출금 트랜잭션만을 지연시키는 것에 해당합니다. 자세한 내용은 [여기](https://docs.optimism.io/stack/protocol/rollup/overview)에서 확인할 수 있습니다.
- **챌린지 프로세스**: 대부분의 Optimistic Rollup에서 챌린지는 permissioned entity (예: 밸리데이터)만 제기할 수 있도록 구현되어 있습니다. fraud proof가 명백히 악의적인 목적으로 제기되면, 거버넌스나 security council 등 상위 권한의 시스템에 의해 밸리데이터에서 추방되거나 슬래싱됩니다. 자세한 내용은 [여기](https://blog.kroma.network/about-the-first-successful-challenge-on-kroma-mainnet-aeca715b05d7)에서 확인할 수 있습니다.

### 참고

- Optimistic Rollup의 finality는 1주일로, 1주일 동안의 분쟁이 가능하여 L2 네트워크의 속도에는 영향을 주지 않습니다. (악의적인 challenge 요청 자체가)
- 이의 제기 과정은 여러 트랜잭션으로 나누어 진행되며, 담보는 미리 예치해야 하고 단번에 예치와 출금을 수행할 수 없습니다. 가장 직관적인 방법은 L1 컨트랙트에서 L2 트랜잭션을 실행하여 output을 확인하는 것이지만, 계산 리소스가 많이 소모되므로 bisect 과정을 통해 아주 작은 부분을 검증하여 승자와 패자를 결정합니다.

자세한 내용은 [여기](https://kelvinfichter.com/pages/thoughts/challenge-periods/)에서 확인할 수 있습니다.

### References

- [Optimistic Rollup 설명](https://docs.optimism.io/stack/protocol/rollup/overview)
- [Optimistic Rollup의 작동 원리](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/)
- [Gemini의 Optimistic Rollup 설명](https://www.gemini.com/cryptopedia/search?query=optimistic-rollup)
- [Optimistic Rollup Tutorial](https://ethereum.org/en/developers/tutorials/optimism-std-bridge-annotated-code/)
- [Kroma 체인의 첫 성공적인 챌린지 사례](https://blog.kroma.network/about-the-first-successful-challenge-on-kroma-mainnet-aeca715b05d7)
- [Challenge Periods에 대한 설명](https://kelvinfichter.com/pages/thoughts/challenge-periods/)

### Related Keywords

- [[Rollup]]
- [[Layer2]]
- [[Blockchain]]
- [[Scalability]]
- [[Security]]
