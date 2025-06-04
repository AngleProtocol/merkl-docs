---
description: Basics about Merkl Incentive mechanisms and customizability features
---

# 🪷 Merkl Incentive Mechanisms

Merkl is designed to be highly versatile, supporting a wide range of incentive structures across both DeFi and broader Web3 ecosystems.

## 🔧 Customization Options

When creating a campaign on Merkl, incentive providers can configure:

### 1️⃣ Campaign Type

[Campaign types](./campaigns/README.md) define the action that needs to be done to be eligible to rewards.

Merkl supports multiple campaign types, rewarding users for both financial activities (e.g., liquidity provision, lending) and other onchain/offchain actions.

Some of the most commonly used campaign types include

- [**Concentrated Liquidity Incentives**](./campaigns/concentrated-liquidity-mechanisms.md): Rewards liquidity providers (LPs) in Concentrated Liquidity AMMs (CLAMMs) such as Uniswap V3.
- [**Lending & Borrowing Incentives**](./campaigns/lending-borrowing.md): Encourages activity on lending protocols like Morpho, Silo, and Radiant, or rewards specific behaviors within these protocols.
- [**Airdrop Campaigns**](./campaigns/airdrop.md): Distributes tokens to a potentially millions of users based on either a JSON file or a snapshotted token balance.
- [**ERC20 Holder Incentives**](./campaigns/erc20-mechanisms.md): Rewards users for holding an ERC20 token over time. This can be used for:
  - Lending & Borrowing LP Rewards: incentivizing liquidity providers who hold receipt or debt tokens on lending protocols
  - Constant Product Liquidity Pools: distributing rewards based on LP token balances to boost liquidity in traditional AMMs like Uniswap V2
  - Holding Incentives: Rewarding users who hold a token like wBTC, ETH, USDA, EURA, etc. based on their balance over time

### 2️⃣ Distribution Type

[Distribution types](./distributions/README.md) define how rewards are allocated among eligible users. Common models include:

- Variable APR campaigns: Rewards are distributed proportionally based on liquidity provided relative to others.
- Fixed APR campaigns: Rewards accrue at a fixed rate for a given amount of liquidity.

## 🔄 Feature Compatibility

Not all campaign types are compatible with all distribution types. Similarly, some hooks may only work with specific campaign or distribution types.

We are actively building a feature compatibility page within the Merkl app to display which campaign types support specific distribution methods and hooks.

### How To Track Merkl's Current Feature set

Until this page is live, you can refer to:

- [Merkl Integration Page](https://app.merkl.xyz/integrations): Lists supported chains, reward tokens, as well as integrated AMMs, and ALMs.
- [Merkl Create Campaign Page](https://studio.merkl.xyz): gives the list of campaign types supported in the frontend

Some hook, campaign type, and distribution combinations are not configurable directly via the frontend. In such cases, we recommend reaching out to us—we can either configure your campaign for you or provide dedicated API endpoints to help you set it up.

{% hint style="info" %}
Merkl’s capabilities continuously expand, adding support for new campaign types, distribution methods, and hooks.
The pages above may not always reflect the latest features. If you need a custom incentivization model, contact us by opening a [BD ticket on Discord](https://discord.gg/jnYfrGxDbe) or sending a message on Telegram.
{% endhint %}
