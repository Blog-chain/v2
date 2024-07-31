---
title: Proof of Stake
description: 블록체인 네트워크에서 지분 증명(Proof of Stake)의 개념, 역할, 및 중요성을 다룹니다.
aliases: [proof of stake, PoS, blockchain proof of stake]
tags: [technology, blockchain, consensus, proof of stake]
date: 2024-07-22
---
### Summary

`Proof of Stake (PoS)`는 블록체인 네트워크에서 특정 양의 암호화폐를 보유한 사용자가 블록을 생성하고 검증하는 합의 메커니즘이다.

Validator가 네트워크에 지분을 스테이킹하여 트랜잭션(혹은 블록)을 검증, 또는 블록을 생성하는 이더리움의 합의 매커니즘이다. 지분 스테이킹에 대한 대가로 수수료를 받을 수 있으며, 부정직하게 행동할 경우 스테이킹된 Ether를 슬래싱([[Slashing]])당할 수 있다.

### Description

`Proof of Stake (PoS)`는 블록체인 네트워크에서 중요한 역할을 합니다. 이는 특정 양의 암호화폐를 보유한 사용자가 블록을 생성하고 검증하는 합의 메커니즘으로, 네트워크의 보안을 유지하고 에너지 소비를 줄이는 데 중요한 역할을 합니다. PoS의 주요 특징은 다음과 같습니다:

1. **지분 기반 블록 생성**: PoS에서는 블록 생성자가 자신의 지분을 바탕으로 블록을 생성합니다. 이는 채굴자 대신 검증자가 블록을 생성하는 방식입니다.
2. **에너지 효율성**: PoS는 PoW와 달리 복잡한 수학 문제를 해결할 필요가 없으므로 에너지 소비가 적습니다. 이는 환경 친화적인 합의 메커니즘입니다.
3. **보안 강화**: PoS는 네트워크의 보안을 강화합니다. 검증자는 자신의 지분을 위험에 노출시켜 네트워크의 무결성을 유지합니다.


![[Validator#How this works]] 

# How transaction gets executed

---

1. user가 transaction에 privatekey를 이용해 서명

   → wallet또는 library에 의해 작동 or RPC를 이용

   → user는 validator로 하여금 트랜잭션을 block에 포함하도록 할 수 있는 동기를 주는 Gas limit을 정함

   → Base fee는 Burn되고, 나머지가 전달됨. → Base fee에 대해 알아보자

2. transaction은 node의 execution client로 전송되어 트랜잭션의 validity를 검증받음.

   → sender에게 transaction을 실행시킬 수 있는 충분한 ETH가 있는지 검증

   → 올바른 key를 이용해 서명했는지 검증

3. execution client는 이 트랜잭션들을 local mempool에 추가함(list of pending transactions)
4. execution layer gossip network의 다른 노드들에게 트랜잭션을 broadcast함
5. 이를 수신한 다른 노드들은 트랜잭션을 본인들의 local mempool에 추가&broadcast

   → 이 과정에서 어떠한 user는 broadcasting을 하는 것 보다는 Flashbot Auction에 전달함.

   → 이를 통해 MEV를 할 수 있음

   → 추가정보: [이더리움-MEV](https://ethereum.org/en/developers/docs/mev/#mev-extraction)

6. 각 slot에서는 RANDAO 프로토콜을 통해 pseudo-random하게 block proposer가 선출되게 된다

   → 이 block proposer는 다음에 생성될 블록을 만들고, 이더리움 블록체인에 broadcasting하며, global state를 변경시키는 역할을 맡는다.

7. block proposer는 위에서도 언급되었던 execution client에서, local mempool에 있는 트랜잭션들을 “execution payload”의 형태로 묶고, 이를 locally execute하여 state change를 생성한다.
8. 이 정보는 consensus client로 넘어가게 되는데, 거기에서는 execution payload, reward, penalties, slashings, attestation 등 에 대한 정보가 “beacon block”의 형태로 묶이게 된다.

   → 추가적인 정보: [이더리움-client간 연결](https://ethereum.org/en/developers/docs/networking-layer/#connecting-clients)

9. 다른 노드들은 consensus layer gossip network에서 새로운 beacon block을 수신하여 이를 그들의 execution client로 넘겨주게 된다.

   → execution client에서는 execution payload가 locally execute되어 proposed state change가 타당한지에 대한 검사를 진행하게 된다.

10. node의 validator client는 체인에 대한 자신의 관점(local state)에서 블록이 유효함을 증언(attest)하는 메시지를 브로드캐스트한다.

    → 이를 통해 block은 각 노드의 로컬 블록체인에 추가된다.

# Finality

(Gasper파트에서 좀 더 자세히 다룰 예정)

트랜잭션은 블록에 추가되어 Finality를 가지게 되면, 엄청난 수의 ETH를 burn시키지 않고서는 내용을 바꿀 수 없다. PoS 이더리움에서, 이는 체크포인트 블록을 통해 관리된다. 각 에폭(32블록)의 첫 번째 블록이 체크포인트가 되며, Validator들은 Valid하다고 생각하는 체크포인트 Pair에 투표를 하게 된다. 잠깐, 왜 체크포인트 Pair일까?

![[PoS_explain.png]]

이렇게 각 에폭의 가장 첫 블록이 checkpoint가 되며, 체크포인트 쌍이 유효하다고 판단되면 Validator는 해당 쌍에 투표를 하게 되고, 2/3 이상의 투표를 얻게 되면 해당 쌍은 Justified로 업그레이드 된다. justified가 되면, 해당 에폭 이전에 있던 “이미” justified되었던 에폭은 Finalize되어 불변성을 보장받게 된다.

### Finalize된 블록을 Revert하려면?

이미 Finalize된 block을 Revert하게 된다면, 공격자는 Stake되어있는 ETH의 최소 1/3 이상을 잃게될 수 있다.

이유 → [이더리움 공식 블로그](https://blog.ethereum.org/2016/05/09/on-settlement-finality/)

그리고, 공격자는 1/3 이상의 Stake ETH를 매수하여 네트워크가 블록을 Finalize시키지 못하도록 할 수 있는데(2/3이상 투표를 할 수 없게 하는 방식), Ethereum에서는 “Inactivity Leak”이라는 방법을 통해 이를 막을 수 있다. 이는, 4 에폭동안 block이 Finalize되지 않는다면, 과반수 투표에 반대표를 던진 Validator의 Stake된 ETH를 슬래싱하여 다시 건전한 Validator들로 2/3 과반수를 복구해낼 수 있다.

# Fork Choice Rule

모든 네트워크가 건전, 정직하게 작동한다면, 모든 블록의 뒤에 추가되는 블록은 fork없이 단 한 개의 블록만 생성될 것이다. 그러나, Validator들은 네트워크 지연의 이유로 인해 서로 다른 관점(view)를 가지게 될 수 있다. 이렇게 되면, consensus client들은 어느 블록을 지지, 또는 선호하는지에 대한 합의를 이루어야 한다. Ethereum PoS에서는 이를 LMD-GHOST를 이용해 해결하고자 하며, 이는 가장 많은 가중치를 할당받은 Fork를 인정하는 방식으로 Fork Choice를 한다.

### Gasper, Casper, LMD-GHOST에 대한 추가 설명

##### Ethereum 기반의 PoS 기술

- **LMD-GHOST (Latest Message Driven - Greedy Heaviest Observed SubTree)**: 이 알고리즘은 검증자들이 최신 메시지를 기반으로 가장 무거운 서브트리를 선택하여 포크 선택 규칙을 결정한다. 이는 네트워크의 안정성과 보안을 강화하는 데 중요한 역할을 한다.
- **Casper**: Ethereum의 PoS 구현 중 하나로, 검증자가 블록을 생성하고 합의에 도달하는 과정을 정의한다. Casper는 네트워크의 보안을 강화하고, 이중 지불 공격을 방지하는 데 중요한 역할을 한다.



### References

- [이더리움 공식 블로그](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)
- [Proof of Stake 설명](https://en.bitcoin.it/wiki/Proof_of_Stake)
- [PoS의 작동 원리](https://ethereum.org/en/developers/docs/consensus-mechanisms/pos/)
- [Ethereum/glossary/PoS](https://ethereum.org/en/glossary#pos)

### Related Keywords

- [[Consensus]]
- [[Blockchain]]
- [[Security]]
- [[Energy Efficiency]]
- [[LMD-GHOST]]
- [[Casper]]
