---
description: Basics about Merkl Incentive mechanisms and campaign design.
---

# Incentive Mechanisms

Merkl is designed to be highly versatile, supporting a wide range of incentive structures across both DeFi and broader Web3 ecosystems.

## 🔧 Campaign Design

When creating a campaign on Merkl, campaign creators can configure:

### 1️⃣ Campaign Type

[Campaign types](../mechanisms/campaigns/) define the action that needs to be done to be eligible to rewards.

Merkl supports multiple campaign types, rewarding users for both financial activities (e.g., liquidity provision, lending) and other onchain/offchain actions.

Some of the most commonly used campaign types include

* [**Concentrated Liquidity**](campaign-types/concentrated-liquidity-mechanisms.md): Rewards liquidity providers (LPs) in Concentrated Liquidity AMMs (CLAMMs) such as Uniswap V3.
* [**Lending & Borrowing**](campaign-types/lending-borrowing.md): Encourages activity on lending protocols like Morpho, Silo, and Radiant, or rewards specific behaviors within these protocols.
* [**Airdrop**](campaign-types/airdrop.md): Distributes tokens to a potentially millions of users based on either a JSON file or a snapshotted token balance.
* [**Token Holding**](campaign-types/erc20-mechanisms.md): Rewards users for holding an ERC20 token over time. This can be used for:
  * Lending & Borrowing LP Rewards: incentivizing liquidity providers who hold receipt or debt tokens on lending protocols
  * Constant Product Liquidity Pools: distributing rewards based on LP token balances to boost liquidity in traditional AMMs like Uniswap V2
  * Holding Incentives: Rewarding users who hold a token like wBTC, ETH, USDA, EURA, etc. based on their balance over time

### 2️⃣ Distribution Type

[Distribution types](distributions.md) define how rewards are allocated among eligible users. Common models include:

* [Variable reward rate campaigns](distributions.md#variable-reward-rate-campaigns): rewards are distributed proportionally based on time-weighted liquidity within the eligibility pool.
* [Fixed reward rate campaigns](distributions.md#fixed-reward-rate-campaigns): a predefined amount of rewards per unit of liquidity is distributed at a fixed rate.
* [Capped reward rate campaigns](distributions.md#capped-reward-rate-campaigns): similar to variable rate campaigns, but with a maximum APR that cannot be exceeded.

## 🔄 Feature Compatibility

{% hint style="danger" %}
Not all campaign types are compatible with all distribution types. Similarly, some customization options may only work with specific campaign or distribution types.
{% endhint %}

### How to Track Merkl's Current Feature Set

To check the status of Merkl’s features, the compatibility between campaign types and distribution types, or to view the list of supported chains and tokens, simply visit [**Merkl Studio**](https://studio.merkl.xyz/) and simulate the creation of a campaign.

To check whether a smart contract is detected as a [forwarder](features.md#forwarders) by Merkl, go to the [**Forwarder Scan page**](https://forwarders.merkl.xyz/), accessible from the navigation bar in Merkl Studio by clicking 'Scan'.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-10-10 à 15.20.07 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Some [customization options](customization-options.md), [campaign types](campaign-types/), and [distribution types](distributions.md) are not configurable directly via Merkl Studio. In such cases, we recommend reaching out to us — we can either configure your campaign for you or provide dedicated API endpoints to help you set it up.
{% endhint %}

{% hint style="success" %}
Merkl’s capabilities continuously expand, adding support for new campaign types, distribution methods, and customization options. If you need a custom incentivization model, contact us by opening a [BD ticket on Discord](https://discord.gg/jnYfrGxDbe) or sending a message on Telegram.
{% endhint %}
