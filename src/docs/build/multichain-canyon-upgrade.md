---
title: Upgrade Guide - Multi-Chain Node & Canyon Upgrade Guide
lang: en-US
---

::: info Update Your Software for these benefits

This upgrade allows nodes to coordinate hard forks together and is intended for chain operators, node operators, and sequence providers.

:::


## Overview

We expect numerous OP Chains to be governed by the [Law of Chains](https://optimism.mirror.xyz/JfVOJ1Ng2l5H6JbIAtfOcYBKa4i9DyRTUJUuOqDpjIs). Our long-term vision is to unite them all as part of a single, unified Superchain that provides unlimited scalability while appearing to users as a single chain. 
This isn’t the case today, however; OP Chains are currently deployed and upgraded separately. As a first step towards achieving Superchain unification, we need to make the operation of each chain as simple as possible. 
This will lay the foundation for future iterations of the Superchain’s user experience.

This upgrade guide covers two essential parts:
- Multi-chain Node Upgrade
- Canyon Hard Fork Upgrade

## Multi-chain Node Upgrade

This release enshrines a `protocol-version` within nodes as a constant in the software, with requirements and recommendations signaled through the L1 `ProtocolVersions` contract. Nodes periodically read the value from L1 and compare it against the locally hardcoded constant. 
This release also introduces a "Superchain target" (within the [superchain-registry](https://github.com/ethereum-optimism/superchain-registry), a library the op-node uses) which groups chains together.

Each chain operator is still in charge of upgrading their own sequencer nodes, but their nodes will now be aware of upgrades, and can be configured to halt or produce warnings when an upgrade is near and the node is behind the protocol-version. 
Because this behavior is optional, this is not a consensus-critical change for this release. However, we anticipate future releases to utilize this functionality more explicitly and upgrade the Superchain in unison.

`rollup.json` and `genesis.json` are deprecated for chains that follow a Superchain target, and will be imported into the op-node from the [superchain-registry](https://github.com/ethereum-optimism/superchain-registry) instead.

### Release

This release includes changes to the superchain-registry and setting mainnet and sepolia superchain-target protocol version contract addresses.
Feel free to upgrade testnets and replicas first to test out this change.

`op-node`                                              
| Flag | Parameter | Description | Default / Recommended value |
| --- | --- | --- | --- |
| --network value | ($OP_NODE_NETWORK) | Predefined network selection. Available networks: op-mainnet, op-goerli, op-sepolia | xx |
| --beta.extra-networks | ($OP_NODE_BETA_EXTRA_NETWORKS) | Enable selection of a predefined-network from the superchain-registry. The availability of configurations in the superchain-registry may change. Available networks: zora-goerli, base-mainnet, pgn-mainnet, zora-mainnet, op-sepolia, pgn-sepolia, base-goerli, op-goerli, op-labs-chaosnet-0-goerli-dev-0, op-labs-devnet-0-goerli-dev-0, op-mainnet  | (default: false)  |
| --rollup.halt value  | ($OP_NODE_ROLLUP_HALT) | Opt-in option to halt on incompatible protocol version requirements of the given level (major/minor/patch/none), as signaled onchain in L1  | xx |
| --rollup.load-protocol-versions | ($OP_NODE_ROLLUP_LOAD_PROTOCOL_VERSIONS) | Load protocol versions from the superchain L1 ProtocolVersions contract (if available), and report in logs and metrics | (default: false) |

`op-geth`
| Flag | Parameter | Description | Default / Recommended value |
| --- | --- | --- | --- |
| --rollup.halt value | ($GETH_ROLLUP_HALT) | Opt-in option to halt on incompatible protocol version requirements of the given level (major/minor/patch/none), as signaled through the Engine API by the rollup node | xx |
| --op-network value | ($GETH_BETA_OP_NETWORK) | Pick an OP Stack network configuration  | xx |
| --rollup.superchain-upgrades | ($GETH_BETA_ROLLUP_SUPERCHAIN_UPGRADES) | Apply superchain-registry config changes to the local chain-configuration | (default: false) |

### Recommended Node Configuration Values
This example uses OP-Sepolia but can **also be configured with ENV vars on both op-node and op-geth!**

`op-node`
--network=op-sepolia
--beta.extra-networks   # if not running op-sepolia/mainnet/goerli
--rollup.halt=minor     # if opting in to halting on incompatible breaking changes
--rollup.load-protocol-versions=true

`op-geth`
--rollup.halt=minor
--op-network=op-sepolia
--rollup.superchain-upgrades=true

## Canyon Hard Fork Upgrade



## Next Steps



