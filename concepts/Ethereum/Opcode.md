---
title: Opcode
description: 이더리움 네트워크에서 스마트 계약을 실행하기 위한 기계어 명령어인 EVM opcode의 개념, 역할, 및 중요성을 다룹니다.
aliases: [opcode, evm opcode, ethereum opcode]
tags: [technology, blockchain, ethereum, opcode]
date: 2024-07-22
---

## Opcode

### Summary

`EVM opcode`는 이더리움 네트워크에서 스마트 계약을 실행하기 위한 기계어 명령어입니다.

### Description

`EVM opcode`는 이더리움 네트워크에서 스마트 계약을 실행하기 위한 기계어 명령어로, EVM이 스마트 계약의 코드를 이해하고 실행할 수 있게 해줍니다. Opcode는 스마트 계약의 로직을 구현하는 데 필수적인 요소이며, 가스 소비, 메모리 관리, 흐름 제어 등 다양한 작업을 수행합니다.

### 중요성

- **가스 소비 계산**: 각 opcode는 실행 시 일정량(변동성을 가진 연산도 존재)의 가스를 소비합니다. 이는 네트워크의 자원을 사용하는 대가로, 스마트 계약의 효율성을 평가하는 데 중요한 요소입니다.
- **스마트 계약 실행**: opcode는 스마트 계약의 비즈니스 로직을 실행하는 데 필요한 기본적인 구성 요소입니다.
- **보안**: 일부 opcode는 보안 관련 작업을 수행하여 스마트 계약을 안전하게 유지합니다.

### 주요 Opcode 예시

- **SSTORE (0x55)**: `storage[key] := val`
- **SLOAD (0x54)**: `storage[key]`
- **CREATE (0xF0)**: `addr = keccak256(rlp([address(this), this.nonce]))`
- **CREATE2 (0xF5)**: `addr = keccak256(0xff ++ address(this) ++ salt ++ keccak256(mem[ost:ost+len-1]))[12:]`
  - ++ 연산자는 EVM의 CREATE2 opcode 컨텍스트에서 문자열 또는 바이트 시퀀스의 연결을 나타냅니다.

### Tips

- `a, b => a + b`는 `ADD` opcode가 스택에서 두 항목(`a`와 `b`)을 가져와 이들의 합을 스택에 다시 놓는다는 의미입니다. 가장 왼쪽 항목(`a`)이 스택의 최상위 항목입니다.
- 모든 스택 설명은 이후 스택 항목을 생략합니다. 명시되지 않은 스택 요소는 연산의 의미에 영향을 주지 않는다고 가정할 수 있습니다. 단, 스택 오버플로우가 발생하는 경우는 예외입니다.
- 스택의 최대 크기는 **1024** 항목이며, 모든 스택 항목은 **32** 바이트입니다.
- `a // b`는 내림 나눗셈을 나타냅니다. EVM에서 0으로 나누기의 결과는 0입니다.
- 모든 산술 연산은 모듈로 2\*\*256로 수행됩니다.
- 지정된 `INVALID` opcode를 포함하여 EVM은 현재 **141** 개의 opcode를 구현하고 있으며, 그 중 **65** 개는 중복된 명령어(`PUSHn`, `DUPn`, `SWAPn`, `LOGn`)입니다.

### References

- [evm-playground](https://www.evm.codes/)
- [ethereum/opcodes](https://ethereum.org/en/developers/docs/evm/opcodes/)
- [jellopaper](https://jellopaper.org/evm/)
- [opcodes/gas](https://github.com/wolflo/evm-opcodes/blob/main/gas.md)

### Related Keywords

- [[Ethereum]]
- [[EVM]]
- [[Smart Contract]]
- [[Solidity]]
- [[Gas]]
- [[Security]]
