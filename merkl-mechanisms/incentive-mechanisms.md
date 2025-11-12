---
description: Basics about Merkl Incentive mechanisms and campaign design.
---

# Incentive Mechanisms

Merkl is designed to be highly versatile, supporting a wide range of incentive structures across both DeFi and broader Web3 ecosystems.

## 🔧 Campaign Design

When creating a campaign on Merkl, campaign creators can configure:

### 1️⃣ Campaign Type

[Campaign types](../merkl-mechanisms/campaign-types/README.md) define the action that needs to be done to be eligible to rewards.

Merkl supports multiple campaign types, rewarding users for both financial activities (e.g., liquidity provision, lending) and other onchain/offchain actions.

Some of the most commonly used campaign types include

* [**Concentrated Liquidity**](campaign-types/concentrated-liquidity-mechanisms.md): Rewards liquidity providers (LPs) in Concentrated Liquidity AMMs (CLAMMs) such as Uniswap V3 or Uniswap V4.
* [**Lending & Borrowing**](campaign-types/lending-borrowing.md): Encourages activity on lending protocols like Morpho, Euler, or Aave, or rewards specific behaviors within these protocols.
* [**Airdrop**](campaign-types/airdrop.md): Distributes tokens to a potentially millions of users based on either a JSON file or a snapshotted token balance.
* [**Token Holding**](campaign-types/erc20-mechanisms.md): Rewards users for holding an ERC20 token over time. This can be used for virtually any protocol that gives ERC20 receipt tokens to their stakeholders.

### 2️⃣ Distribution Type

[Distribution types](distributions.md) define the **reward distribution model** and **how the total campaign budget is spent over time**. Common models include:

* [Variable reward rate campaigns](distributions.md#variable-reward-rate-campaigns): rewards are distributed proportionally based on time-weighted liquidity within the eligibility pool.
* [Fixed reward rate campaigns](distributions.md#fixed-reward-rate-campaigns): a predefined amount of rewards per unit of liquidity is distributed at a fixed rate.
* [Capped reward rate campaigns](distributions.md#capped-reward-rate-campaigns): similar to variable rate campaigns, but with a maximum APR that cannot be exceeded.

### 3️⃣ Scoring Type

[Scoring types](scoring.md) define **how individual user contributions are measured and converted into reward shares**. Common models include:

* **1:1 mapping (default)**: User reward share equals their share of total contributions
* **Max balance scoring**: Caps eligible balances by taking the minimum between a user's time-weighted balance and a predefined maximum

### 4️⃣ Customization Options

Merkl supports a wide range of [customization options](customization-options.md) to further personalize campaigns beyond core settings. These include filters to dynamically restrict or boost the eligible users for a campaign based on whether they hold a token.

## 🔄 Feature Compatibility

Not all campaign types are compatible with all distribution types. Similarly, some customization options may only work with specific campaign or distribution types.

To check the status of Merkl’s features, the compatibility between campaign types, distribution or scoring types, or to view the list of supported chains and tokens, simply visit [**Merkl Studio**](https://studio.merkl.xyz/) and simulate the creation of a campaign.

{% hint style="info" %}
Some [customization options](customization-options.md), [campaign types](campaign-types/), [distribution types](distributions.md) and [scoring types](scoring.md) are not configurable directly via Merkl Studio. In such cases, we recommend reaching out to us — we can either configure your campaign for you or provide dedicated API endpoints to help you set it up.
{% endhint %}

{% hint style="success" %}
Merkl’s capabilities continuously expand, adding support for new campaign types, distribution methods, and customization options. If you need a custom incentivization model, contact us by opening a [BD ticket on Discord](https://discord.gg/jnYfrGxDbe) or sending a message on Telegram.
{% endhint %}
