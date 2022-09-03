---
id: versions
title: History
sidebar_label: History of changes
description: "Deployed versions"
keywords:
  - docs
  - polygon
  - nightfall
  - changelog
image: https://matic.network/banners/matic-network-16x9.png
---


## Beta 1.0
Beta 1.0 was released in May 17th, 2022. It included the basic mechanism to have Nightfall proof of concept deployed.
It supported a single `Proposer` and first `coarse` implementation of transactions. It also provided a preliminary
version of a user wallet.
Deployed version can be found [here](https://github.com/EYBlockchain/nightfall_3/commit/bc3e475de3e2877f14430f9599e5b38ea960765b)

## Beta 2.0
Deployed version can be found [here](https://github.com/EYBlockchain/nightfall_3/commit/4c2af01ac95af5ea6f5b40071d73a1624f06ba46)
Several improvements have been made:
- **Collecting Fees** 

Ability for proposers to collect fees for transactions the collect. This feature will incentivize multiple proposer.
- **Challenger Deployment**

Network now includes a challenger monitoring the proposer's blocks.
- **Circuits Transactions**

More flexibility and better performance when generating the ZKP of any transactions.  Both `transfer` and `withdrawal` accept up to two commitments and return change.
Regarding performance, transactions now are optimized so that it takes shorter time to proof.

| Operations| Constraints before | Constrains after | 
|-----------|--------------------|------------------|
| Deposit   | 84,766             |  1277            |
| Transfer  | 499,119	         | 67,694           |
| Withdraw  |  168,883           | 53,348           |

Part of the optimization has been possible by switching to Poseidon

- **Secrets encryption** 

We have moved from El-Gamal to a [KEM-DEM](../protocol/secrets) encryption process which has resulted in several benefits:
- Simplification of the encryption/decryption process
- Reduction in the constraints and on-chain gas costs. 

- **Challenger deployment**

