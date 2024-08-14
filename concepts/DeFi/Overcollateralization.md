---
title: Overcollateralization
description: 과다 담보의 개념을 다룹니다.
aliases: [과다 담보, over-collateralization]
tags: [finance, defi]
date: 2024-08-12
---

### Summary

`Overcollateralization` is the provision of collateral that is worth more than enough to cover potential losses in cases of default. It is a safety mechanism used to reduce the credit risk of the borrower. If the borrower fails to repay the loan, the lender can liquidate the collateral to recover the loan amount.

### Description

대출에서 `Collateralization`은 대출액에 상응하는 가치를 가진 무언가로, 채무자가 대출금을 상환하지 못할 경우 채권자가 대출금을 상환하기 위한 안전장치로 사용됩니다.

`Overcollateralization`은 이러한 담보의 가치가 대출금보다 더 많은 가치를 가지고 있는 상태를 의미합니다. 이는 DeFi에서 주로 사용되는 개념으로, 가상자산의 변동성이 높은 특성을 고려한 조치입니다. UniSwap, MakerDAO, Compound 등의 프로토콜에서는 대출금의 150% 이상의 담보물을 요구하는 경우가 많습니다.

단, flash loan은 단일 트랜잭션 내에 상환되는 대출을 의미하며, 트랜잭션의 결과로 대출금 상환이 이루어지지 않으면 대출 전체가 취소되는 특성을 가지고 있습니다. 따라서 flash loan은 담보 자체를 일반적으로 요구하지 않습니다.

### References

- [Over-Collateralization (OC): Definition, Benefits, and Examples](https://www.investopedia.com/terms/o/overcollateralization.asp)
- [Overcollateralization 설명](https://en.wikipedia.org/wiki/Overcollateralization)

### Related Keywords

- [[Finance]]
- [[DeFi]]
- [[Stablecoin]]
- [[LTV]]
