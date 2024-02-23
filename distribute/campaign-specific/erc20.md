---
description: Guide for people using Merkl to incentivize ERC20 holders
---

# Incentivize ERC20 tokens with Merkl

Everything you need to know to make the most of your incentives on concentrated liquidity pools!

## Campaign configuration

The following parameters are specific to ERC20 campaigns, you can find the parameters common to all campaigns [here](../README.md#configure-your-campaign)

### Required Parameters

- Token Address: address of the token you want to incentivize

### Optional Parameters

- Forwarders: list of Merkl forwarders to forward rewards accrued by smart contracts to end users. This has not yet been implemented in the frontend, if you wish to add forwarders you should get in touch with us on Telegram

## Use cases

### Incentivize using any protocol which gives users an ERC20 token when depositing liquidity

**Goal:** reward users who use your protocol

This can be applied to:

- Lending protocols (e.g. Radiant, Aave, Silo etc.)
- Strategies protocols (e.g. Yearn)

**Key configuration parameters**:

- Token address: address of the token received when depositing liquidity
