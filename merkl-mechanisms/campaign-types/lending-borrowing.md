---
description: Reward users who lend, borrow, or use tokens as collateral in lending markets
---

# Lending and Borrowing Campaigns

## 🫴🏼 Supported protocols

Merkl supports **most major lending and borrowing protocols**, such as Morpho, Euler, Compound, and Aave.

The full list of supported protocols can be found in the ["Lending & Borrowing" section of Merkl Studio](https://studio.merkl.xyz/create-campaign/lend) when selecting a protocol.

<figure><img src="../../.gitbook/assets/Group 14.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Merkl natively supports **all lending and borrowing protocols that issue receipt tokens** for lenders and debt tokens for borrowers — such as Aave with aTokens.

For these protocols, please use the [“Token Holding” incentive category](https://studio.merkl.xyz/create-campaign/erc20) in Merkl Studio, as you are incentivizing a token that represents a lending position.
{% endhint %}

{% hint style="info" %}
If you’d like your lending and borrowing protocol to be fully integrated and supported by Merkl — or if you're looking to create tailored protocol-specific campaigns — please reach out on the [Merkl Discord](https://discord.com/invite/jnYfrGxDbe) by opening a BD ticket to discuss the integration process.
{% endhint %}

## 🔁 Preventing lending loops

Merkl allows campaign creators to reward users based on their **net lending position** (lending minus borrowing) to avoid lending and borrowing loops.

As a campaign creator, you have the possibility to enable net lending on a specific opportunity. When enabled, users will not get extra rewards by borrowing the same asset and re-lending it again. This is to ensure there are no infinite loops & unefficient TVL.

For example, if you lend $100 wstETH and borrow $60 wstETH, you get rewarded for $40 wstETH ($100 - $60).

Note: this is not always the case. If you’ve got a doubt about net lending when preparing a campaign, feel free to reach out to us.

## 🪡 Campaign personalization

Campaign creators can design custom lending incentive campaigns by leveraging:

* [distribution types](../distributions.md): fixed reward rate, variable reward rate, …
* [customization options](../customization-options.md): whitelist / blacklist systems, boosts,…
* [additional features](../features.md): forwarders, token wrappers,…

## 🚀 Create a campaign

To set up and launch a lending & borrowing incentive campaign, please follow the steps in the [“Create a Campaign”](../../distribute-with-merkl/create-a-campaign.md) section.

## 🦋 Morpho multi-market campaign

With Merkl you can **incentivize a single token (e.g. USDC) across all Morpho markets with just one campaign!**

No need to precise a specific markets or vault to incentivize.

Merkl detects all Morpho markets where the selected incentivized token is active and also identifies every wallet that interacted with those markets. Then, Merkl automatically cascades rewards across every whitelisted Morpho markets where the token is used based on:

* the liquidity and size of each market
* the user’s share of liquidity

To create a multi-market campaign, you just need to select any of these three options when selection an action to incentivize in the Choose target step in Merkl Studio:

* `Supply a token to any market`
* `Borrow a token from any market`
* `Use a token as collateral on any market`

<figure><img src="../../.gitbook/assets/Group 15.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
**Who is this designed for?**

The multi-market campaign feature is especially aimed at **token issuers**, such as stablecoin protocols. It allow them to grow their token easily by running a campaign across all Morpho markets — for example, rewarding DeFi users who use their token as collateral.
{% endhint %}
