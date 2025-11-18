---
description: Step-by-step guide for creating a campaign using Merkl Studio
---

# Create a Campaign Using Merkl Studio

Merkl Studio is your command center to launch, manage, and optimize incentive campaigns. This guide walks you through the process of creating a campaign step by step.

<figure><img src="../.gitbook/assets/Capture d'écran 2025-06-10 à 17.41.52 1.png" alt=""><figcaption><p>Merkl Studio homepage</p></figcaption></figure>

{% hint style="info" %}
Merkl Studio doesn't support all campaign types and customization options offered by Merkl. If you need features not available in the Studio or have questions, please reach out to us.
{% endhint %}

## Step-by-Step Guide

### 1. Connect Your Wallet

Connect the wallet holding the rewards you want to distribute. Ensure you're connected to the chain where you want to distribute rewards. To see all supported chains, check the [Status page](https://app.merkl.xyz/status) in the Merkl App.

<figure><img src="../.gitbook/assets/Group 8.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Capture d'écran 2025-06-10 à 17.42.03 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
You can create cross-chain campaigns with Merkl—incentivize an asset on one chain while distributing rewards on another (e.g., incentivize a WETH-USDC pool on Ethereum by distributing WBTC on Base).
{% endhint %}

### 2. Click Create Campaign

Click the _Create Campaign_ button to begin setting up your campaign.

<figure><img src="../.gitbook/assets/Group 7.png" alt=""><figcaption></figcaption></figure>

### 3. Select Your Incentive Category

Choose your incentive category by clicking on the appropriate card.

<figure><img src="../.gitbook/assets/Capture d'écran 2025-06-10 à 17.42.21 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To incentivize Aave lending positions, Uniswap V2, or similar liquidity pools, use the _Token Holding_ campaign category.
{% endhint %}

### 4. Configure Your Rewards

Fill out campaign details including the asset to incentivize, the asset type (token or point), the reward amount to distribute, start date, end date, and more. The required details depend on the campaign type you selected. For detailed explanations of each parameter, see the [campaign types page](../merkl-mechanisms/campaign-types/).

<figure><img src="../.gitbook/assets/Group 18.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The total rewards amount represents the total to be distributed over the entire campaign duration (a maintenance fee may be applied on top).
{% endhint %}

{% hint style="info" %}
Merkl sets a minimum token amount that can be distributed per hour for each reward token. If your token amount is too low (typically less than $1 per hour), you will not be able to create your campaign.
{% endhint %}

### 5. Set Restrictions

This section allows you to configure blacklists and/or whitelists for your campaign. Blacklisted addresses won't receive any rewards, while whitelisted addresses will be the only addresses eligible for rewards (excluding all others).

<figure><img src="../.gitbook/assets/Group 27.png" alt=""><figcaption></figcaption></figure>

### 6. Personalize Your Campaign

Campaigns on Merkl can be fully customized to your needs. You can apply customization options such as boosts and more. New options are constantly being added! Check out the [Customization options page](../merkl-mechanisms/customization-options.md) for a comprehensive list of available options.

<figure><img src="../.gitbook/assets/Group 20.png" alt=""><figcaption></figcaption></figure>

You may also be asked to provide a deposit URL—the link where users can interact with the asset you want to incentivize (e.g., a lending market, liquidity pool, etc.). Since Merkl is non-custodial, users must interact directly with protocols to be eligible for rewards. Merkl does not hold any user funds.

### 7. Review Campaign and Accept Terms

Double-check all the information. Once you're ready, click the _Accept Terms_ button and confirm in your wallet.

<figure><img src="../.gitbook/assets/Group 9.png" alt=""><figcaption></figcaption></figure>

### 8. Approve and Launch

Approve the reward token transfer from your wallet to Merkl, then create the campaign by signing the transaction. You can sign and submit using either an EOA (externally owned account) or a multisig wallet.

Regardless of the method you choose, you will need to complete these 3 steps:

* Accept the Terms & Conditions
* Approve the tokens for transfer
* Deposit the tokens

#### Using an EOA:

* Double-check your campaign configuration
* Read and accept Merkl's Terms & Conditions by clicking the _Accept_ button and signing with your wallet
* Approve the tokens for transfer and deposit the amount you want to distribute, plus [the standard Merkl fees](../distribute-with-merkl/fee-model.md)

#### Using a Multisig Wallet (Safe)

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. To learn how to deploy your campaign from a multisig, see [this guide](./create-your-campaign-from-a-multisig-or-gnosis-safe.md).

## Campaign Launch Complete

Congratulations! You have successfully launched your incentive campaign! 🎉

Your campaign may take up to **one hour** to become visible on the frontend after creation.

{% hint style="note" %}
It's expected for your campaign's **TVL and APR to show 0** at launch, and for the **leaderboard to appear empty**. The Merkl Engine needs a few hours to perform [the necessary computations](../merkl-mechanisms/technical-overview.md#reward-computation) before this data becomes available.
{% endhint %}
