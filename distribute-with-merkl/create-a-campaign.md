---
description: Everything you need to know to create campaigns on Merkl
---

# Create a campaign

## 🪜 Step-by-step process for single campaign creation

1. **Go to Merkl Studio**\
   [Merkl Studio](https://studio.merkl.xyz/) is your command center to launch, manage and optimize your incentive campaigns.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.41.52 1.png" alt=""><figcaption><p>Merkl Studio homepage</p></figcaption></figure>

2. **Connect your wallet**

Connect the wallet holding the rewards to distribute. Make sure you’re connected on the chain on which you want to distribute the rewards. To see all chains that Merkl supports, check the [Status page](https://app.merkl.xyz/status) in the Merkl App.

<figure><img src="../.gitbook/assets/Group 8.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.42.03 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
You can create cross-chain campaigns with Merkl — incentivize an asset on one chain while distributing the rewards on another (e.g. incentivize an WETH-USDC pool on Ethereum by distributing WBTC on Base).
{% endhint %}

3. **Click on&#x20;**_**Create Campaign**_

Click on the _Create Campaign_ button to create a single campaign.

<figure><img src="../.gitbook/assets/Group 7.png" alt=""><figcaption></figcaption></figure>

4. **Select your incentive category**

Choose your incentive category by clicking on the appropriate card.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-06-10 à 17.42.21 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
To incentivize Aave lending positions, Uniswap V2, or similar liquidity pools, please use the _Token Holding_ campaign category.
{% endhint %}

5. **Configure your rewards**

Fill out campaign details such as the asset to incentivize, the asset type (token or point), the reward amount to distribute, a start date and an end date, etc. The details required will depend on the campaign type you selected. Please check the [campaign types page](../merkl-mechanisms/campaign-types/) to get detailed explanations of each required parameter.

<figure><img src="../.gitbook/assets/Group 18.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The total rewards amount is the total amount of rewards to be distributed over the whole campaign duration (a maintenance fee may be applied on top).
{% endhint %}

{% hint style="info" %}
For each token to distribute as reward, Merkl sets a minimum amount of token that can be distributed per hour. If your token amount is too low (generally parameters are calibrated so that you cannot distribute less than $1 per hour), then you will not be able to create your campaign.
{% endhint %}

6. **Set restrictions**

This section allows you to set blacklist and/or whitelist for your campaign. Blacklisted addresses won't receive any rewards, whereas whitelisted addresses will exclusively receive the campaign's rewards (excluding all other addresses).

<figure><img src="../.gitbook/assets/Group 27.png" alt=""><figcaption></figcaption></figure>

7. **Personalize your campaign**

Campaigns on Merkl can be fully tailored to your needs. You can choose whether or not to apply customization options such as boosts, and more. New options are constantly being added to Merkl! Check out the non-exhaustive list of options on the [Customization options page](../merkl-mechanisms/customization-options.md).

<figure><img src="../.gitbook/assets/Group 20.png" alt=""><figcaption></figcaption></figure>

You may also be asked to provide a deposit URL — this is the link to the asset you want to incentivize (e.g. a lending market, liquidity pool, etc.). Since Merkl is non-custodial, users must interact directly with the protocols to be eligible for rewards. Merkl does not hold any user funds.

8. **Review campaign and accept Terms**

Double-check all the information. Once you’re done, hit the _Accept Terms_ button, and confirm in your wallet.

<figure><img src="../.gitbook/assets/Group 9.png" alt=""><figcaption></figcaption></figure>

9. **Approve and launch**

Approve rewards transfer from your wallet to Merkl, then create the campaign by signing the transaction. You can sign and submit using either an EOA account or a multisig wallet.

Regardless of the method you choose, you will need to follow these 3 steps:

* accept the T\&C
* approve the tokens for transfer
* deposit the tokens

{% hint style="info" %}
**Using an EOA Account:**

* Double-check your campaign configuration.
* Read and accept Merkl's T\&Cs by clicking on the _Accept_ button and signing using your wallet.
* Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.
{% endhint %}

{% hint style="info" %}
**Using a multisig wallet (Safe Wallet):**

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](broken-reference/) where everything is explained in more detail.
{% endhint %}

Congratulations! You have launched your incentive campaign! 🎉

{% hint style="success" %}
**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**
{% endhint %}

## 🧪 Test campaigns

You may want to start testing the flow and integrating our data before launching your campaign. To learn more about test campaigns and how to use our test token (aglaMerkl), please refer to the [Before you start](https://docs.merkl.xyz/distribute-with-merkl/before-you-start#test-campaigns) section.
