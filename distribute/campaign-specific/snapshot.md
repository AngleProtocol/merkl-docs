---
description: Guide for people using Merkl to reward users based on a snapshot of their token balance
---

# Snapshots with Merkl

Everything you need to know to take snapshots with Merkl!

## Campaign configuration

The following parameters are specific to snapshot campaigns, you can find the parameters common to all campaigns [here](../README.md#configure-your-campaign)

### Required Parameters

- Snapshot date: date at which the snapshot will be taken, Merkl will use the first block prior to the date provided. This date should be in the past.
- Distribution date: date at which the rewards will be available in Merkl. This date should be in the future
- Token Address: address of the token that will be snapshoted

### Optional Parameters

- Forwarders: list of Merkl forwarders to forward rewards accrued by smart contracts to end users. This has not yet been implemented in the frontend, if you wish to add forwarders you should get in touch with us on Telegram.

## Use cases

### Reward holders of a governance token

**Goal:** reward users who held your token at a given date

**Key configuration parameters**:

- Token address: address of the governance token
- Snapshot date: date at which the snapshot will be taken
