PoW에서 PoS로 전환

존재 이유: 왜 도입됐는지
어떤 효과를 거두었는지
동작 원리: 무엇을 어떻게 동작시키는지

- The Beacon Chain introduced proof-of-stake to the Ethereum ecosystem.
- It was merged with the original Ethereum proof-of-work chain in September 2022.
- The Beacon Chain introduced the consensus logic and block gossip protocol which now secures Ethereum.

https://ethereum.org/ko/roadmap/beacon-chain/

The merge 이후 Beacon Chain은 Consensus 네트워크가 되었고, 

기존 네트워크도 버리는 건 아니고
original clients from the exe
gossiping and executing transactions, and managing Ethereum's state
하는 데, 통신을 위해 Engine API를 사용한다고 한다.


## Engine API

https://hackmd.io/@danielrachi/engine_api

https://github.com/ethereum/execution-apis/tree/main/src/engine

https://hackmd.io/@danielrachi/engine_api

Engine API Implementation


evmVersion
https://docs.blockscout.com/for-developers/information-and-settings/evm-version-information






# The Merge

The Merge 변천사
1. 비트코인 위에 프로그램을 실행시킬 수 있도록 하자 제안했지만 거절 당함 
2. 그렇게 PoW 기반의 Ethereum으로 따로 만들기 시작
3. 추가로 검증 차원에서 별도로 Beacon Chain을 가동
4. PoW의 보상 시스템보다, PoS의 처벌 시스템이 보안에 더 유리하다고 생각
5. 당시 사용하던 PoW 네트워크는 (Transaction) Execution Layer로 사용하고, Beacon Chain은 Consensus Layer로 사용
6. Execution 레이어는 Paris, Consensus 레이어는 Bellatrix 버전에서 “The Merge”를 기점으로 PoS 전환 (두 네트워크는 Engine API 스펙에 맞춰 통신)
7. 당시 Eth 1, Eth 2 등의 용어가 있었지만 현재는 `Ethereum`으로 통합


원래 존재하던 비콘 체인이 The Merge를 시점으로 PoW 블록 체인과 병합되었다
-> 근데 어떻게 병합함? (어떻게 상태를 유지하지?)
-> hard-fork를 해야 한다면, 새로운 네트워크를 개설하고, 해당 네트워크에 기존 네트워크의 deposit을 옮길 수 있도록 하나?
아직 이루어지지 않은 샤딩은 무엇일까?

이는 hard-fork로서, 기존 PoW 방식의 Consensus rule이 변경 이후 Compatability를 지원하지 않아 발생되었다. 
https://www.reddit.com/r/ethereum/comments/wrgmq8/is_the_merge_a_soft_fork/?rdt=57745

The Merge was executed on September 15, 2022. This completed Ethereum's transition to proof-of-stake consensus, officially deprecating proof-of-work and reducing energy consumption by ~99.95%.

![[Pasted image 20240708103504.png]]




-----

기존 PoW 네트워크는 이제 Execution layer로 변경되었다고 하니,
아래의 스펙에서 동작하는 
Ethereum execution client specifications
https://github.com/ethereum/execution-specs


The merge가 적용된 버전은 `Paris`라고 합니다. [evm-version-information](https://docs.blockscout.com/for-developers/information-and-settings/evm-version-information)

해당 링크를 보면 다양한 역대 evm을 확인할 수 있는데, `Engine API` 버전과 유사하면서도 다른 점이 있습니다. [execution-apis](https://github.com/ethereum/execution-apis/blob/main/src/engine/paris.md)

바로 `Gray Glacier` 이전의 버전들이 없다는 것인데요 (사진 1), 이로서도 분명하게 `paris`를 기점으로 hard-fork 되었음을 확인할 수 있을 것 같습니다.

그럼 `The Merge` 이전의 코드는 어디서 관리되고 있는 걸까요?

hard-fork 라 하면 수정된 각각의 버전들을 총칭하는 말이라는 생각도 든다.
hard-forking이라 하면 되려나, 그냥 기존의 하드 포크에 수정을 가할 때, compatibility 가 없으면 hard-fork의 새로운 버전이 되고

Solidity
solc
opcode

\


https://github.com/ethereum/execution-specs

https://docs.soliditylang.org/en/latest/using-the-compiler.html#setting-the-evm-version-to-target

EVM
EVM은 단순히 Opcode의 집합일까?

1. https://docs.blockscout.com/for-developers/information-and-settings/evm-version-information





### Ethereum Proof-of-Stake Consensus Specifications
https://github.com/ethereum/consensus-specs
phase0 -> Bellatrix (The Merge) -> Deneb

| Seq. | Code Name                                                                   | Fork Epoch | Specs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---- | --------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0    | **Phase0**                                                                  | `0`        | - Core<br><br>- [The beacon chain](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/beacon-chain.md)<br>- [Deposit contract](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/deposit-contract.md)<br>- [Beacon chain fork choice](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/fork-choice.md)<br><br>- Additions<br><br>- [Honest validator guide](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/validator.md)<br>- [P2P networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/p2p-interface.md)<br>- [Weak subjectivity](https://github.com/ethereum/consensus-specs/blob/dev/specs/phase0/weak-subjectivity.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 1    | **Altair**                                                                  | `74240`    | - Core<br><br>- [Beacon chain changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/beacon-chain.md)<br>- [Altair fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/fork.md)<br><br>- Additions<br><br>- [Light client sync protocol](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/sync-protocol.md) ([full node](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/full-node.md), [light client](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/light-client.md), [networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/light-client/p2p-interface.md))<br>- [Honest validator guide changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/validator.md)<br>- [P2P networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/altair/p2p-interface.md)                                                                                                                                                                                                                          |
| 2    | **Bellatrix**  <br>(["The Merge"](https://ethereum.org/en/upgrades/merge/)) | `144896`   | - Core<br><br>- [Beacon Chain changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/beacon-chain.md)<br>- [Bellatrix fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/fork.md)<br>- [Fork choice changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/fork-choice.md)<br><br>- Additions<br><br>- [Honest validator guide changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/validator.md)<br>- [P2P networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/p2p-interface.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| 3    | **Capella**                                                                 | `194048`   | - Core<br><br>- [Beacon chain changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/beacon-chain.md)<br>- [Capella fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/fork.md)<br><br>- Additions<br><br>- [Light client sync protocol changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/light-client/sync-protocol.md) ([fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/light-client/fork.md), [full node](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/light-client/full-node.md), [networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/light-client/p2p-interface.md))<br><br>- [Validator additions](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/validator.md)<br>- [P2P networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/capella/p2p-interface.md)                                                                                                                                                                                                                                |
| 4    | **Deneb**                                                                   | `269568`   | - Core<br><br>- [Beacon Chain changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/beacon-chain.md)<br>- [Deneb fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/fork.md)<br>- [Polynomial commitments](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/polynomial-commitments.md)<br>- [Fork choice changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/fork-choice.md)<br><br>- Additions<br><br>- [Light client sync protocol changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/light-client/sync-protocol.md) ([fork](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/light-client/fork.md), [full node](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/light-client/full-node.md), [networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/light-client/p2p-interface.md))<br><br>- [Honest validator guide changes](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/validator.md)<br>- [P2P networking](https://github.com/ethereum/consensus-specs/blob/dev/specs/deneb/p2p-interface.md) |


## Engine API


### Ethereum Execution Client 
frontier -> Paris (The Merge) -> Cancun

| Version and Code Name | Block No. | Released                     | Incl EIPs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Specs                                                                                                                                                                             | Blog                                                                                                     |
| --------------------- | --------- | ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Cancun                | TBD       | TBD                          | [EIP-1153](https://eips.ethereum.org/EIPS/eip-1153)  <br>[EIP-4788](https://eips.ethereum.org/EIPS/eip-4788)  <br>[EIP-4844](https://eips.ethereum.org/EIPS/eip-4844)  <br>[EIP-5656](https://eips.ethereum.org/EIPS/eip-5656)  <br>[EIP-6780](https://eips.ethereum.org/EIPS/eip-6780)  <br>[EIP-7516](https://eips.ethereum.org/EIPS/eip-7516)                                                                                                                                                          | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/cancun.md)                                                              | TBD                                                                                                      |
| Shanghai              | 17034870  | 2023-04-12  <br>(1681338455) | [EIP-3651](https://eips.ethereum.org/EIPS/eip-3651)  <br>[EIP-3855](https://eips.ethereum.org/EIPS/eip-3855)  <br>[EIP-3860](https://eips.ethereum.org/EIPS/eip-3860)  <br>[EIP-4895](https://eips.ethereum.org/EIPS/eip-4895)                                                                                                                                                                                                                                                                            | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/shanghai.md)                                                            | [Blog](https://blog.ethereum.org/2023/03/28/shapella-mainnet-announcement)                               |
| Paris                 | 15537394  | 2022-09-15                   | [EIP-3675](https://eips.ethereum.org/EIPS/eip-3675)  <br>[EIP-4399](https://eips.ethereum.org/EIPS/eip-4399)                                                                                                                                                                                                                                                                                                                                                                                              | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/paris.md)                                                               | [Blog](https://blog.ethereum.org/2022/08/24/mainnet-merge-announcement)                                  |
| Gray Glacier          | 15050000  | 2022-06-30                   | [EIP-5133](https://eips.ethereum.org/EIPS/eip-5133)                                                                                                                                                                                                                                                                                                                                                                                                                                                       | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/gray-glacier.md)                                                        | [Blog](https://blog.ethereum.org/2022/06/16/gray-glacier-announcement/)                                  |
| Arrow Glacier         | 13773000  | 2021-12-09                   | [EIP-4345](https://eips.ethereum.org/EIPS/eip-4345)                                                                                                                                                                                                                                                                                                                                                                                                                                                       | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/arrow-glacier.md)                                                       | [Blog](https://blog.ethereum.org/2021/11/10/arrow-glacier-announcement/)                                 |
| London                | 12965000  | 2021-08-05                   | [EIP-1559](https://eips.ethereum.org/EIPS/eip-1559)  <br>[EIP-3198](https://eips.ethereum.org/EIPS/eip-3198)  <br>[EIP-3529](https://eips.ethereum.org/EIPS/eip-3529)  <br>[EIP-3541](https://eips.ethereum.org/EIPS/eip-3541)  <br>[EIP-3554](https://eips.ethereum.org/EIPS/eip-3554)                                                                                                                                                                                                                   | [Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/london.md)                                                              | [Blog](https://blog.ethereum.org/2021/07/15/london-mainnet-announcement/)                                |
| Berlin                | 12244000  | 2021-04-15                   | [EIP-2565](https://eips.ethereum.org/EIPS/eip-2565)  <br>[EIP-2929](https://eips.ethereum.org/EIPS/eip-2929)  <br>[EIP-2718](https://eips.ethereum.org/EIPS/eip-2718)  <br>[EIP-2930](https://eips.ethereum.org/EIPS/eip-2930)                                                                                                                                                                                                                                                                            | ~~[HFM-2070](https://eips.ethereum.org/EIPS/eip-2070)~~  <br>[Specification](https://github.com/ethereum/execution-specs/blob/master/network-upgrades/mainnet-upgrades/berlin.md) | [Blog](https://blog.ethereum.org/2021/03/08/ethereum-berlin-upgrade-announcement/)                       |
| Muir Glacier          | 9200000   | 2020-01-02                   | [EIP-2384](https://eips.ethereum.org/EIPS/eip-2384)                                                                                                                                                                                                                                                                                                                                                                                                                                                       | [HFM-2387](https://eips.ethereum.org/EIPS/eip-2387)                                                                                                                               | [Blog](https://blog.ethereum.org/2019/12/23/ethereum-muir-glacier-upgrade-announcement/)                 |
| Istanbul              | 9069000   | 2019-12-07                   | [EIP-152](https://eips.ethereum.org/EIPS/eip-152)  <br>[EIP-1108](https://eips.ethereum.org/EIPS/eip-1108)  <br>[EIP-1344](https://eips.ethereum.org/EIPS/eip-1344)  <br>[EIP-1884](https://eips.ethereum.org/EIPS/eip-1884)  <br>[EIP-2028](https://eips.ethereum.org/EIPS/eip-2028)  <br>[EIP-2200](https://eips.ethereum.org/EIPS/eip-2200)                                                                                                                                                            | [HFM-1679](https://eips.ethereum.org/EIPS/eip-1679)                                                                                                                               | [Blog](https://blog.ethereum.org/2019/11/20/ethereum-istanbul-upgrade-announcement/)                     |
| Petersburg            | 7280000   | 2019-02-28                   | [EIP-145](https://eips.ethereum.org/EIPS/eip-145)  <br>[EIP-1014](https://eips.ethereum.org/EIPS/eip-1014)  <br>[EIP-1052](https://eips.ethereum.org/EIPS/eip-1052)  <br>[EIP-1234](https://eips.ethereum.org/EIPS/eip-1234)                                                                                                                                                                                                                                                                              | [HFM-1716](https://eips.ethereum.org/EIPS/eip-1716)                                                                                                                               | [Blog](https://blog.ethereum.org/2019/02/22/ethereum-constantinople-st-petersburg-upgrade-announcement/) |
| Constantinople        | 7280000   | 2019-02-28                   | [EIP-145](https://eips.ethereum.org/EIPS/eip-145)  <br>[EIP-1014](https://eips.ethereum.org/EIPS/eip-1014)  <br>[EIP-1052](https://eips.ethereum.org/EIPS/eip-1052)  <br>[EIP-1234](https://eips.ethereum.org/EIPS/eip-1234)  <br>[EIP-1283](https://eips.ethereum.org/EIPS/eip-1283)                                                                                                                                                                                                                     | [HFM-1013](https://eips.ethereum.org/EIPS/eip-1013)                                                                                                                               | [Blog](https://blog.ethereum.org/2019/02/22/ethereum-constantinople-st-petersburg-upgrade-announcement/) |
| Byzantium             | 4370000   | 2017-10-16                   | [EIP-100](https://eips.ethereum.org/EIPS/eip-100)  <br>[EIP-140](https://eips.ethereum.org/EIPS/eip-140)  <br>[EIP-196](https://eips.ethereum.org/EIPS/eip-196)  <br>[EIP-197](https://eips.ethereum.org/EIPS/eip-197)  <br>[EIP-198](https://eips.ethereum.org/EIPS/eip-198)  <br>[EIP-211](https://eips.ethereum.org/EIPS/eip-211)  <br>[EIP-214](https://eips.ethereum.org/EIPS/eip-214)  <br>[EIP-649](https://eips.ethereum.org/EIPS/eip-649)  <br>[EIP-658](https://eips.ethereum.org/EIPS/eip-658) | [HFM-609](https://eips.ethereum.org/EIPS/eip-609)                                                                                                                                 | [Blog](https://blog.ethereum.org/2017/10/12/byzantium-hf-announcement/)                                  |
| Spurious Dragon       | 2675000   | 2016-11-22                   | [EIP-155](https://eips.ethereum.org/EIPS/eip-155)  <br>[EIP-160](https://eips.ethereum.org/EIPS/eip-160)  <br>[EIP-161](https://eips.ethereum.org/EIPS/eip-161)  <br>[EIP-170](https://eips.ethereum.org/EIPS/eip-170)                                                                                                                                                                                                                                                                                    | [HFM-607](https://eips.ethereum.org/EIPS/eip-607)                                                                                                                                 | [Blog](https://blog.ethereum.org/2016/11/18/hard-fork-no-4-spurious-dragon/)                             |
| Tangerine Whistle     | 2463000   | 2016-10-18                   | [EIP-150](https://eips.ethereum.org/EIPS/eip-150)                                                                                                                                                                                                                                                                                                                                                                                                                                                         | [HFM-608](https://eips.ethereum.org/EIPS/eip-608)                                                                                                                                 | [Blog](https://blog.ethereum.org/2016/10/13/announcement-imminent-hard-fork-eip150-gas-cost-changes/)    |
| DAO Fork              | 1920000   | 2016-07-20                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | [HFM-779](https://eips.ethereum.org/EIPS/eip-779)                                                                                                                                 | [Blog](https://blog.ethereum.org/2016/07/15/to-fork-or-not-to-fork/)                                     |
| DAO Wars              | aborted   | aborted                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                   | [Blog](https://blog.ethereum.org/2016/06/24/dao-wars-youre-voice-soft-fork-dilemma/)                     |
| Homestead             | 1150000   | 2016-03-14                   | [EIP-2](https://eips.ethereum.org/EIPS/eip-2)  <br>[EIP-7](https://eips.ethereum.org/EIPS/eip-7)  <br>[EIP-8](https://eips.ethereum.org/EIPS/eip-8)                                                                                                                                                                                                                                                                                                                                                       | [HFM-606](https://eips.ethereum.org/EIPS/eip-606)                                                                                                                                 | [Blog](https://blog.ethereum.org/2016/02/29/homestead-release/)                                          |
| Frontier Thawing      | 200000    | 2015-09-07                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                   | [Blog](https://blog.ethereum.org/2015/08/04/the-thawing-frontier/)                                       |
| Frontier              | 1         | 2015-07-30                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                   | [Blog](https://blog.ethereum.org/2015/07/22/frontier-is-coming-what-to-expect-and-how-to-prepare/)       |

## Network-upgrades > Mainnet-upgrades
https://github.com/ethereum/execution-specs/blob/master/network-upgrades/README.md
Homestead -> Paris (The Merge) -> Cancun

| Name                        | Date         |
| --------------------------- | ------------ |
| Homestead                   | 14 Mar 2016  |
| Tangerine Whistle           | 18 Oct 2016  |
| Spurious Dragon             | 22 Nov 2016  |
| Byzantium                   | 16 Oct 2017  |
| Constantinople / Petersburg | 28 Feb 2019  |
| Istanbul                    | 08 Dec 2019  |
| Berlin                      | 14 Apr 2021  |
| London                      | 05 Aug 2021  |
| _Arrow Glacier*_            | 09 Aug 2021  |
| _Gray Glacier*_             | 30 June 2022 |
| Paris (The Merge)           | 15 Sep 2022  |
| Shanghai                    | 12 Apr 2023  |
| Cancun                      | 13 Mar 2024  |



## Existing Opcodes
Shanghai

|Number|Name|Execution spec category|Initial Release|EIP|
|---|---|---|---|---|
|0x00|STOP|Control Flow|||
|0x01|ADD|Arithmetic|||
|0x02|MUL|Arithmetic|||
|0x03|SUB|Arithmetic|||
|0x04|DIV|Arithmetic|||
|0x05|SDIV|Arithmetic|||
|0x06|MOD|Arithmetic|||
|0x07|SMOD|Arithmetic|||
|0x08|ADDMOD|Arithmetic|||
|0x09|MULMOD|Arithmetic|||
|0x0A|EXP|Arithmetic|||
|0x0B|SIGNEXTEND|Arithmetic|||
|0x10|LT|Comparison|||
|0x11|GT|Comparison|||
|0x12|SLT|Comparison|||
|0x13|SGT|Comparison|||
|0x14|EQ|Comparison|||
|0x15|ISZERO|Comparison|||
|0x16|AND|Bitwise|||
|0x17|OR|Bitwise|||
|0x18|XOR|Bitwise|||
|0x19|NOT|Bitwise|||
|0x1A|BYTE|Bitwise|||
|0x1B|SHL|Bitwise|Constantinople|[EIP-145](https://eips.ethereum.org/EIPS/eip-145)|
|0x1C|SHR|Bitwise|Constantinople|[EIP-145](https://eips.ethereum.org/EIPS/eip-145)|
|0x1D|SAR|Bitwise|Constantinople|[EIP-145](https://eips.ethereum.org/EIPS/eip-145)|
|0x20|KECCAK|Keccak|||
|0x30|ADDRESS|Environmental|||
|0x31|BALANCE|Environmental|||
|0x32|ORIGIN|Environmental|||
|0x33|CALLER|Environmental|||
|0x34|CALLVALUE|Environmental|||
|0x35|CALLDATALOAD|Environmental|||
|0x36|CALLDATASIZE|Environmental|||
|0x37|CALLDATACOPY|Environmental|||
|0x38|CODESIZE|Environmental|||
|0x39|CODECOPY|Environmental|||
|0x3A|GASPRICE|Environmental|||
|0x3B|EXTCODESIZE|Environmental|||
|0x3C|EXTCODECOPY|Environmental|||
|0x3D|RETURNDATASIZE|Environmental|Byzantium|[EIP-211](https://eips.ethereum.org/EIPS/eip-211)|
|0x3E|RETURNDATACOPY|Environmental|Byzantium|[EIP-211](https://eips.ethereum.org/EIPS/eip-211)|
|0x3F|EXTCODEHASH|Environmental|Constantinople|[EIP-1052](https://eips.ethereum.org/EIPS/eip-1052)|
|0x40|BLOCKHASH|Block|||
|0x41|COINBASE|Block|||
|0x42|TIMESTAMP|Block|||
|0x43|NUMBER|Block|||
|0x44|DIFFICULTY|Block|Frontier->London||
|0x44|PREVRANDAO|Block|Paris|[EIP-4399](https://eips.ethereum.org/EIPS/eip-4399)|
|0x45|GASLIMIT|Block|||
|0x46|CHAINID|Block|Istanbul|[EIP-1344](https://eips.ethereum.org/EIPS/eip-1344)|
|0x47|SELFBALANCE|Block|Istanbul|[EIP-1884](https://eips.ethereum.org/EIPS/eip-1884)|
|0x48|BASEFEE|Block|London|[EIP-3198](https://eips.ethereum.org/EIPS/eip-3198)|
|0x50|POP|Pop|||
|0x51|MLOAD|Memory|||
|0x52|MSTORE|Memory|||
|0x53|MSTORE8|Memory|||
|0x54|SLOAD|Storage|||
|0x55|SSTORE|Storage|||
|0x56|JUMP|Control Flow|||
|0x57|JUMPI|Control Flow|||
|0x58|PC|Control Flow|||
|0x59|MSIZE|Memory|||
|0x5A|GAS|Control Flow|||
|0x5B|JUMPDEST|Control Flow|||
|0x5F|PUSH0|Push|Shanghai|[EIP-3855](https://eips.ethereum.org/EIPS/eip-3855)|
|0x60|PUSH1|Push|||
|0x61|PUSH2|Push|||
|0x62|PUSH3|Push|||
|0x63|PUSH4|Push|||
|0x64|PUSH5|Push|||
|0x65|PUSH6|Push|||
|0x66|PUSH7|Push|||
|0x67|PUSH8|Push|||
|0x68|PUSH9|Push|||
|0x69|PUSH10|Push|||
|0x6A|PUSH11|Push|||
|0x6B|PUSH12|Push|||
|0x6C|PUSH13|Push|||
|0x6D|PUSH14|Push|||
|0x6E|PUSH15|Push|||
|0x6F|PUSH16|Push|||
|0x70|PUSH17|Push|||
|0x71|PUSH18|Push|||
|0x72|PUSH19|Push|||
|0x73|PUSH20|Push|||
|0x74|PUSH21|Push|||
|0x75|PUSH22|Push|||
|0x76|PUSH23|Push|||
|0x77|PUSH24|Push|||
|0x78|PUSH25|Push|||
|0x79|PUSH26|Push|||
|0x7A|PUSH27|Push|||
|0x7B|PUSH28|Push|||
|0x7C|PUSH29|Push|||
|0x7D|PUSH30|Push|||
|0x7E|PUSH31|Push|||
|0x7F|PUSH32|Push|||
|0x80|DUP1|Dup|||
|0x81|DUP2|Dup|||
|0x82|DUP3|Dup|||
|0x83|DUP4|Dup|||
|0x84|DUP5|Dup|||
|0x85|DUP6|Dup|||
|0x86|DUP7|Dup|||
|0x87|DUP8|Dup|||
|0x88|DUP9|Dup|||
|0x89|DUP10|Dup|||
|0x8A|DUP11|Dup|||
|0x8B|DUP12|Dup|||
|0x8C|DUP13|Dup|||
|0x8D|DUP14|Dup|||
|0x8E|DUP15|Dup|||
|0x8F|DUP16|Dup|||
|0x90|SWAP1|Swap|||
|0x91|SWAP2|Swap|||
|0x92|SWAP3|Swap|||
|0x93|SWAP4|Swap|||
|0x94|SWAP5|Swap|||
|0x95|SWAP6|Swap|||
|0x96|SWAP7|Swap|||
|0x97|SWAP8|Swap|||
|0x98|SWAP9|Swap|||
|0x99|SWAP10|Swap|||
|0x9A|SWAP11|Swap|||
|0x9B|SWAP12|Swap|||
|0x9C|SWAP13|Swap|||
|0x9D|SWAP14|Swap|||
|0x9E|SWAP15|Swap|||
|0x9F|SWAP16|Swap|||
|0xA0|LOG0|Log|||
|0xA1|LOG1|Log|||
|0xA2|LOG2|Log|||
|0xA3|LOG3|Log|||
|0xA4|LOG4|Log|||
|0xF0|CREATE|System|||
|0xF1|CALL|System|||
|0xF2|CALLCODE|System|||
|0xF3|RETURN|System|||
|0xF4|DELEGATECALL|System|Homestead|[EIP-7](https://eips.ethereum.org/EIPS/eip-7)|
|0xF5|CREATE2|System|Constantinople|[EIP-1014](https://eips.ethereum.org/EIPS/eip-1014)|
|0xFA|STATICCALL|System|Byzantium|[EIP-214](https://eips.ethereum.org/EIPS/eip-214)|
|0xFD|REVERT|System|Byzantium|[EIP-140](https://eips.ethereum.org/EIPS/eip-140)|
|0xFE|INVALID/ABORT|System|(unofficial)|[EIP-141](https://eips.ethereum.org/EIPS/eip-141)|
|0xFF|SELFDESTRUCT|System||
https://github.com/ethereum/execution-specs/blob/master/lists/evm/pending-opcodes.md
https://github.com/ethereum/execution-specs/blob/master/lists/evm/proposed-opcodes.md


