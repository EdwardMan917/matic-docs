---
id: bls
title: BLS
description: "Explanation and instructions regarding BLS mode."
keywords:
  - docs
  - polygon
  - edge
  - bls
---

## Overview

BLS is a signature scheme that can aggregate multiple signatures. In Polygon Edge, BLS is used by default in order to provide better security in the IBFT consensus mode. BLS can aggregate signatures into a single byte array and reduce the block header size. Each chain can choose whether to use BLS or not. The ECDSA key is used regardless of whether the BLS mode is enabled or not.

## How to setup a new chain using BLS

Refer to the [Local Setup](/docs/edge/get-started/set-up-ibft-locally) / [Cloud Setup](/docs/edge/get-started/set-up-ibft-on-the-cloud) sections for detailed setup instructions.

## How to migrate from an existing ECDSA PoA chain to BLS PoA chain

This section describes how to use the BLS mode in an existing PoA chain.
the following steps are required in order to enable BLS in a PoA chain.

1. Stop all nodes
2. Generate BLS keys for validators
3. Add fork settings into genesis.json
4. Confirm block generatation after the `from` block

### 1. Stop all nodes
Terminate all processes of the validators by pressing Ctrl + c (Control + c). Please remember the latest block height (the highest sequence number in block committed log).

```bash
# Press Ctrl + c

...

[SIGNAL] Caught signal: interrupt
Gracefully shutting down client...
```
### 2. Generate BLS key

As mentioned above, `secrets init` with `--bls` flag can generate the BLS key. In order to keep the existing ECDSA and Network key and add a new BLS key, `--ecdsa` and `--network` need to be disabled.

```bash
polygon-edge secrets init --bls --ecdsa=false --network=false

[SECRETS INIT]
Public key (address) = 0x...
BLS Public key       = 0x...
Node ID              = 16...
```

### 3. Add fork settings

`ibft switch` command adds fork setting, which enables BLS in the middle, into `genesis.json`.

For PoA chain, validators need to be given in the command. As with the way of `genesis` command, `--ibft-validators-prefix-path` or `--ibft-validator` flags can be used to specify the validator.

Specify the height from which the chain starts BLS mode for `--from`.

```bash
polygon-edge ibft switch --chain ./genesis.json --type PoA --ibft-validator-type bls --ibft-validators-prefix-path test-chain- --from 100
```

### 4. Confirm block generatation after the `from` block

Run server command to restart all validators and make sure block production happens after `from` you specified in the previous step.

## How to migrate from an existing ECDSA PoS chain to BLS PoS chain

This section describes how to use the BLS mode in existing PoS chain.
The following steps are required in order to enable BLS in PoS chain.

1. Stop all nodes
2. Generate BLS keys for validators
3. Add fork settings into genesis.json
4. Call the staking contract to register BLS Public Key
5. Confirm block generatation after the `from` block

### 1. Stop all nodes
Terminate all processes of the validators by pressing Ctrl + c (Control + c). Please remember the latest block height (the highest sequence number in block committed log).

```bash
# Press Ctrl + c

...

[SIGNAL] Caught signal: interrupt
Gracefully shutting down client...
```
### 2. Generate BLS key

As mentioned above, `secrets init` with `--bls` flag can generate BLS key. In order to keep existing ECDSA and Network key and add a new BLS key, `--ecdsa` and `--network` need to be disabled.

```bash
polygon-edge secrets init --bls --ecdsa=false --network=false

[SECRETS INIT]
Public key (address) = 0x...
BLS Public key       = 0x...
Node ID              = 16...
```

### 3. Add fork settings

`ibft switch` command adds fork setting, which enables BLS in the middle, into `genesis.json`.

In the PoS chain, `--ibft-validators-prefix-path` or `--ibft-validator` are not used because validator info (address and BLS Public Key) is stored in the staking contract.

Specify the height from which the chain starts BLS mode for `--from`, and the height at which the contract is updated for `--deployment`.

```bash
polygon-edge ibft switch --chain ./genesis.json --type PoS --ibft-validator-type bls --deployment 50 --from 200
```

### 4. Register BLS Public Key in staking contract

After the fork is added and validators are restarted, each validator needs to call staking contract to register BLS Public Key. This must be done after the height specified in `--deployment` before the height specified in `--from`.

The script to register BLS Public Key is defined in [Staking Smart Contract repo](https://github.com/0xPolygon/staking-contracts). 

Set `BLS_PUBLIC_KEY` to be registered into `.env` file. Refer [pos-stake-unstake](/docs/edge/consensus/pos-stake-unstake#setting-up-the-provided-helper-scripts) for more details about other parameters.

```env
JSONRPC_URL=http://localhost:10002
STAKING_CONTRACT_ADDRESS=0x0000000000000000000000000000000000001001
PRIVATE_KEYS=0x...
BLS_PUBLIC_KEY=0x...
```

The following command register the BLS Public Key given in `.env` to the contract.

```bash
npm run register-blskey
```

:::warning Validators need to register BLS Public Key manually
In BLS mode, validators must have their own address and BLS public key. The consensus layer ignores the validators that have not registered BLS public key in the contract when the consensus fetches validators info from the contract.
:::

### 5. Confirm block generatation after the `from` block

Run server command to restart all validators and make sure block production happens after `from` you specified in the previous step.