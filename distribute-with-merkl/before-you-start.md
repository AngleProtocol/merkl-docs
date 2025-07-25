---
description: >-
  You can launch an incentive campaign on your own in just a few minutes using
  Merkl Studio. However, there are a few prerequisites to complete beforehand.
---

# Before you start

## ⚙️ Merkl’s core principles

Before setting up your campaign on Merkl, make sure you have read and understood:

* [**Merkl’s reward distribution process**](../merkl-mechanisms/technical-overview.md) (reward computation vs. reward update, dispute period, etc.)
* [**The different incentive mechanisms**](../merkl-mechanisms/incentive-mechanisms.md) (campaign types, distribution types)
* How to personalize your campaign using [**customization options**](../merkl-mechanisms/customization-options.md)
* [**Additional features**](../merkl-mechanisms/features.md) that can make your campaign unique

{% hint style="info" %}
If you're looking for some incentive mechanism for which you're not sure about whether it's supported, or looking to add incentive features or customization options that are not displayed on the platform, please contact us!
{% endhint %}

Some Merkl features (e.g., new distribution types, campaign types, customization options) may not be directly available in the campaign creation flow. If a required configuration option is not available in the frontend, we can:

* Help you generate the correct campaign payload.
* Provide dedicated API endpoints so you can structure your campaign as needed.

## 🪙 Incentivized assets

Inform us about the asset(s) you want to incentivize. If it’s not deployed yet, deploy it!

## 📝 Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted.

We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users.

You can find all whitelisted tokens by chain on the [status page](https://app.merkl.xyz/status). If your token is not part of the list, please submit the reward token’s price source and logo by filling out this [whitelisting form](https://anglemoney.notion.site/1aecfed0d48c808a8194fe2befd50bdb?pvs=105).

{% hint style="info" %}
If your reward token is not transferable, make sure to whitelist both our campaign creator and distributor contracts for the blockchain where you intend to distribute rewards. You can find the relevant contract addresses on our [status page](https://app.merkl.xyz/status).



![](<../.gitbook/assets/Status page doc.png>)



Note: You need to whitelist the campaignCreator for both transfer and transferFrom!
{% endhint %}

{% hint style="success" %}
Make sure you have all the tokens you want to distribute in your wallet.
{% endhint %}

## 📃 Merkl terms

You must also make sure that you have read and understood [Merkl's Terms & Conditions](https://app.merkl.xyz/terms). You will be required to agree to these terms during the campaign setup process.
