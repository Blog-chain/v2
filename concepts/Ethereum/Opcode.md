---
title: Opcode
description: 이더리움 네트워크에서 스마트 계약을 실행하기 위한 기계어 명령어인 EVM opcode의 개념, 역할, 및 중요성을 다룹니다.
aliases:
  - opcode
  - evm opcode
  - ethereum opcode
tags:
  - blockchain
  - ethereum
  - opcode
date: 2024-07-22
---

# Summary

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

## Existing Opcodes

Shanghai

| Number | Name           | Execution spec category | Initial Release  | EIP                                                 |
| ------ | -------------- | ----------------------- | ---------------- | --------------------------------------------------- |
| 0x00   | STOP           | Control Flow            |                  |                                                     |
| 0x01   | ADD            | Arithmetic              |                  |                                                     |
| 0x02   | MUL            | Arithmetic              |                  |                                                     |
| 0x03   | SUB            | Arithmetic              |                  |                                                     |
| 0x04   | DIV            | Arithmetic              |                  |                                                     |
| 0x05   | SDIV           | Arithmetic              |                  |                                                     |
| 0x06   | MOD            | Arithmetic              |                  |                                                     |
| 0x07   | SMOD           | Arithmetic              |                  |                                                     |
| 0x08   | ADDMOD         | Arithmetic              |                  |                                                     |
| 0x09   | MULMOD         | Arithmetic              |                  |                                                     |
| 0x0A   | EXP            | Arithmetic              |                  |                                                     |
| 0x0B   | SIGNEXTEND     | Arithmetic              |                  |                                                     |
| 0x10   | LT             | Comparison              |                  |                                                     |
| 0x11   | GT             | Comparison              |                  |                                                     |
| 0x12   | SLT            | Comparison              |                  |                                                     |
| 0x13   | SGT            | Comparison              |                  |                                                     |
| 0x14   | EQ             | Comparison              |                  |                                                     |
| 0x15   | ISZERO         | Comparison              |                  |                                                     |
| 0x16   | AND            | Bitwise                 |                  |                                                     |
| 0x17   | OR             | Bitwise                 |                  |                                                     |
| 0x18   | XOR            | Bitwise                 |                  |                                                     |
| 0x19   | NOT            | Bitwise                 |                  |                                                     |
| 0x1A   | BYTE           | Bitwise                 |                  |                                                     |
| 0x1B   | SHL            | Bitwise                 | Constantinople   | [EIP-145](https://eips.ethereum.org/EIPS/eip-145)   |
| 0x1C   | SHR            | Bitwise                 | Constantinople   | [EIP-145](https://eips.ethereum.org/EIPS/eip-145)   |
| 0x1D   | SAR            | Bitwise                 | Constantinople   | [EIP-145](https://eips.ethereum.org/EIPS/eip-145)   |
| 0x20   | KECCAK         | Keccak                  |                  |                                                     |
| 0x30   | ADDRESS        | Environmental           |                  |                                                     |
| 0x31   | BALANCE        | Environmental           |                  |                                                     |
| 0x32   | ORIGIN         | Environmental           |                  |                                                     |
| 0x33   | CALLER         | Environmental           |                  |                                                     |
| 0x34   | CALLVALUE      | Environmental           |                  |                                                     |
| 0x35   | CALLDATALOAD   | Environmental           |                  |                                                     |
| 0x36   | CALLDATASIZE   | Environmental           |                  |                                                     |
| 0x37   | CALLDATACOPY   | Environmental           |                  |                                                     |
| 0x38   | CODESIZE       | Environmental           |                  |                                                     |
| 0x39   | CODECOPY       | Environmental           |                  |                                                     |
| 0x3A   | GASPRICE       | Environmental           |                  |                                                     |
| 0x3B   | EXTCODESIZE    | Environmental           |                  |                                                     |
| 0x3C   | EXTCODECOPY    | Environmental           |                  |                                                     |
| 0x3D   | RETURNDATASIZE | Environmental           | Byzantium        | [EIP-211](https://eips.ethereum.org/EIPS/eip-211)   |
| 0x3E   | RETURNDATACOPY | Environmental           | Byzantium        | [EIP-211](https://eips.ethereum.org/EIPS/eip-211)   |
| 0x3F   | EXTCODEHASH    | Environmental           | Constantinople   | [EIP-1052](https://eips.ethereum.org/EIPS/eip-1052) |
| 0x40   | BLOCKHASH      | Block                   |                  |                                                     |
| 0x41   | COINBASE       | Block                   |                  |                                                     |
| 0x42   | TIMESTAMP      | Block                   |                  |                                                     |
| 0x43   | NUMBER         | Block                   |                  |                                                     |
| 0x44   | DIFFICULTY     | Block                   | Frontier->London |                                                     |
| 0x44   | PREVRANDAO     | Block                   | Paris            | [EIP-4399](https://eips.ethereum.org/EIPS/eip-4399) |
| 0x45   | GASLIMIT       | Block                   |                  |                                                     |
| 0x46   | CHAINID        | Block                   | Istanbul         | [EIP-1344](https://eips.ethereum.org/EIPS/eip-1344) |
| 0x47   | SELFBALANCE    | Block                   | Istanbul         | [EIP-1884](https://eips.ethereum.org/EIPS/eip-1884) |
| 0x48   | BASEFEE        | Block                   | London           | [EIP-3198](https://eips.ethereum.org/EIPS/eip-3198) |
| 0x50   | POP            | Pop                     |                  |                                                     |
| 0x51   | MLOAD          | Memory                  |                  |                                                     |
| 0x52   | MSTORE         | Memory                  |                  |                                                     |
| 0x53   | MSTORE8        | Memory                  |                  |                                                     |
| 0x54   | SLOAD          | Storage                 |                  |                                                     |
| 0x55   | SSTORE         | Storage                 |                  |                                                     |
| 0x56   | JUMP           | Control Flow            |                  |                                                     |
| 0x57   | JUMPI          | Control Flow            |                  |                                                     |
| 0x58   | PC             | Control Flow            |                  |                                                     |
| 0x59   | MSIZE          | Memory                  |                  |                                                     |
| 0x5A   | GAS            | Control Flow            |                  |                                                     |
| 0x5B   | JUMPDEST       | Control Flow            |                  |                                                     |
| 0x5F   | PUSH0          | Push                    | Shanghai         | [EIP-3855](https://eips.ethereum.org/EIPS/eip-3855) |
| 0x60   | PUSH1          | Push                    |                  |                                                     |
| 0x61   | PUSH2          | Push                    |                  |                                                     |
| 0x62   | PUSH3          | Push                    |                  |                                                     |
| 0x63   | PUSH4          | Push                    |                  |                                                     |
| 0x64   | PUSH5          | Push                    |                  |                                                     |
| 0x65   | PUSH6          | Push                    |                  |                                                     |
| 0x66   | PUSH7          | Push                    |                  |                                                     |
| 0x67   | PUSH8          | Push                    |                  |                                                     |
| 0x68   | PUSH9          | Push                    |                  |                                                     |
| 0x69   | PUSH10         | Push                    |                  |                                                     |
| 0x6A   | PUSH11         | Push                    |                  |                                                     |
| 0x6B   | PUSH12         | Push                    |                  |                                                     |
| 0x6C   | PUSH13         | Push                    |                  |                                                     |
| 0x6D   | PUSH14         | Push                    |                  |                                                     |
| 0x6E   | PUSH15         | Push                    |                  |                                                     |
| 0x6F   | PUSH16         | Push                    |                  |                                                     |
| 0x70   | PUSH17         | Push                    |                  |                                                     |
| 0x71   | PUSH18         | Push                    |                  |                                                     |
| 0x72   | PUSH19         | Push                    |                  |                                                     |
| 0x73   | PUSH20         | Push                    |                  |                                                     |
| 0x74   | PUSH21         | Push                    |                  |                                                     |
| 0x75   | PUSH22         | Push                    |                  |                                                     |
| 0x76   | PUSH23         | Push                    |                  |                                                     |
| 0x77   | PUSH24         | Push                    |                  |                                                     |
| 0x78   | PUSH25         | Push                    |                  |                                                     |
| 0x79   | PUSH26         | Push                    |                  |                                                     |
| 0x7A   | PUSH27         | Push                    |                  |                                                     |
| 0x7B   | PUSH28         | Push                    |                  |                                                     |
| 0x7C   | PUSH29         | Push                    |                  |                                                     |
| 0x7D   | PUSH30         | Push                    |                  |                                                     |
| 0x7E   | PUSH31         | Push                    |                  |                                                     |
| 0x7F   | PUSH32         | Push                    |                  |                                                     |
| 0x80   | DUP1           | Dup                     |                  |                                                     |
| 0x81   | DUP2           | Dup                     |                  |                                                     |
| 0x82   | DUP3           | Dup                     |                  |                                                     |
| 0x83   | DUP4           | Dup                     |                  |                                                     |
| 0x84   | DUP5           | Dup                     |                  |                                                     |
| 0x85   | DUP6           | Dup                     |                  |                                                     |
| 0x86   | DUP7           | Dup                     |                  |                                                     |
| 0x87   | DUP8           | Dup                     |                  |                                                     |
| 0x88   | DUP9           | Dup                     |                  |                                                     |
| 0x89   | DUP10          | Dup                     |                  |                                                     |
| 0x8A   | DUP11          | Dup                     |                  |                                                     |
| 0x8B   | DUP12          | Dup                     |                  |                                                     |
| 0x8C   | DUP13          | Dup                     |                  |                                                     |
| 0x8D   | DUP14          | Dup                     |                  |                                                     |
| 0x8E   | DUP15          | Dup                     |                  |                                                     |
| 0x8F   | DUP16          | Dup                     |                  |                                                     |
| 0x90   | SWAP1          | Swap                    |                  |                                                     |
| 0x91   | SWAP2          | Swap                    |                  |                                                     |
| 0x92   | SWAP3          | Swap                    |                  |                                                     |
| 0x93   | SWAP4          | Swap                    |                  |                                                     |
| 0x94   | SWAP5          | Swap                    |                  |                                                     |
| 0x95   | SWAP6          | Swap                    |                  |                                                     |
| 0x96   | SWAP7          | Swap                    |                  |                                                     |
| 0x97   | SWAP8          | Swap                    |                  |                                                     |
| 0x98   | SWAP9          | Swap                    |                  |                                                     |
| 0x99   | SWAP10         | Swap                    |                  |                                                     |
| 0x9A   | SWAP11         | Swap                    |                  |                                                     |
| 0x9B   | SWAP12         | Swap                    |                  |                                                     |
| 0x9C   | SWAP13         | Swap                    |                  |                                                     |
| 0x9D   | SWAP14         | Swap                    |                  |                                                     |
| 0x9E   | SWAP15         | Swap                    |                  |                                                     |
| 0x9F   | SWAP16         | Swap                    |                  |                                                     |
| 0xA0   | LOG0           | Log                     |                  |                                                     |
| 0xA1   | LOG1           | Log                     |                  |                                                     |
| 0xA2   | LOG2           | Log                     |                  |                                                     |
| 0xA3   | LOG3           | Log                     |                  |                                                     |
| 0xA4   | LOG4           | Log                     |                  |                                                     |
| 0xF0   | CREATE         | System                  |                  |                                                     |
| 0xF1   | CALL           | System                  |                  |                                                     |
| 0xF2   | CALLCODE       | System                  |                  |                                                     |
| 0xF3   | RETURN         | System                  |                  |                                                     |
| 0xF4   | DELEGATECALL   | System                  | Homestead        | [EIP-7](https://eips.ethereum.org/EIPS/eip-7)       |
| 0xF5   | CREATE2        | System                  | Constantinople   | [EIP-1014](https://eips.ethereum.org/EIPS/eip-1014) |
| 0xFA   | STATICCALL     | System                  | Byzantium        | [EIP-214](https://eips.ethereum.org/EIPS/eip-214)   |
| 0xFD   | REVERT         | System                  | Byzantium        | [EIP-140](https://eips.ethereum.org/EIPS/eip-140)   |
| 0xFE   | INVALID/ABORT  | System                  | (unofficial)     | [EIP-141](https://eips.ethereum.org/EIPS/eip-141)   |
| 0xFF   | SELFDESTRUCT   | System                  |                  |

https://github.com/ethereum/execution-specs/blob/master/lists/evm/pending-opcodes.md
https://github.com/ethereum/execution-specs/blob/master/lists/evm/proposed-opcodes.md

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
