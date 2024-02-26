---
description: Your official source of information for everything regarding the Merkl platform.
cover: ./.gitbook/assets/merkl-cover.jpg
coverY: 0
---

# 🥨 Merkl Overview

Merkl is a solution to incentivize any complex onchain and offchain behavior. It can not only replicate any type of staking contract, it can also do much more and reward in a tailored manner any onchain or offchain action, or combination of actions.

It is conceived as an agnostic powerhouse enabling everyone to unlock liquidity based on their exact needs without overspending or allocating tokens to reward behavior/users/actions that are not beneficial to the incentivizor. Merkl is for instance much more expressive and efficient at shaping liquidity on concentrated liquidity pools than other solutions available on the market.

Incentives are distributed by deploying **Merkl Campaigns** in which incentivizors select a token to distribute, an amount of that same token and a timeframe over which the token should be distributed. All Merkl campaigns can be found by visiting the [Merkl App](https://merkl.angle.money). Once a campaign is created, rewards are then regularly streamed to eligible liquidity providers through onchain merkle root updates.

For these users, Merkl is a **non custodial solution**. Earning rewards on Merkl incurs **no risk of funds** and requires no specific smart contract interactions. Liquidity Providers can retain the custody of their liquidity while receiving rewards. They only need to interact with Merkl contracts to claim their rewards.

While the set of behaviors that can be incentivized with Merkl is infinite, **Merkl provides the ability to add custom rules to every campaign such as:**

- only rewarding users which matched the eligibility criterias for a set amount of time
- blacklist or whitelist some addresses for every campaigns
- boosting rewards of the users which hold a specific token

Merkl supports many more custom rules which are specific to the type of onchain behavior which is incentivized, it can currently reward:

- liquidity providers on concentrated liquidity protocols (like Uniswap V3)
- holders of a specific ERC20 token for a given amount of time (just like in a staking contract)

{% hint style="info" %}
Merkl offchain logic allows it to easily support new and complex use cases. If there is a specific use case that you are interested in incentivizing efficiently (e.g. users of a perp DEX protocol, borrowers of a lending protocol that are not folding their positions, ...), contact us on [Merkl Discord](https://discord.gg/jnYfrGxDbe).
{% endhint %}

A key feature of Merkl is its ability to trace user activity across protocols. This enables users to accrue rewards even if the incentivized asset isn't directly present in their wallet, the rewards which would have been accrued by a smart contract holding the incentivized asset are forwarded the user which deposited tokens in it (see [Merkl Forwarders](merkl-mechanism#merkl-forwarders))

{% hint style="info" %}
For example, when an ERC20 holder stakes the ERC20 token in a staking contract, the address of the user no longer holds the incentivized ERC20 token. Merkl will detect ERC20 tokens held by the staking contract and forward the rewards to the original user.
{% endhint %}

## ⚱️ Fee Structure

Excluding gas, Merkl is free to use for liquidity providers claiming rewards. There is a maintenance fee of 3% applied to incentives that are sent by incentivizors. This fee can be descreased for large incentivizors.

It can also be waived for incentives which contain some specific approved tokens. In particular, there are no fees for incentives sent to pools which have Angle tokens in it.

{% hint style="info" %}
Merkl is built and maintained by Angle Labs, but is separate from the Angle Protocol.
{% endhint %}
