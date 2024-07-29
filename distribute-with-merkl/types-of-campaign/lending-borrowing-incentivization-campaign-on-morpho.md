---
description: Guide to create a Morpho Campaign
---

# Lending/Borrowing Incentivization Campaign on Morpho

Creating a Morpho campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**
   * Go to the Merkl's App and go to the campaign creation section by selecting *Create Campaign* from the dashboard.

<figure><img src="../../.gitbook/assets/create-campaign-screenshot.png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the *Create Campaign*, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.

<figure><img src="../../.gitbook/assets/whitelist-token-screenshot.png" alt=""><figcaption></figcaption></figure>

3. **Connect you Wallet**

* Connect your wallet and select the chain on which you want to distribute the rewards. **The chain connected to your wallet during the campaign creation will determine where the rewards are distributed.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your Morpho Campaign**

* Once your token is whitelisted, you can create your Morpho campaign by clicking on the *Morpho* button on the campaign creation page (see screenshot below)

<figure><img src="../../.gitbook/assets/morpho-campaign-create.png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/morpho-fill-out-campaign-details.png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to this page (see screenshot above), where you will need to fill out the following information:

**Distribution Details:**

* **Total Rewards:** Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.
* **Duration:** Set the start and end dates for the campaign.
* **Min Rewards/Hour:** Be sure that your distribution per hour is superior than the Min Rewards/Hour. The Min Rewards/Hour is set at $1 per hour.

**Morpho to target:**

* **Chain:** Choose the blockchain network (Ethereum or Base) where the campaign will be conducted. Keep in mind that the chain on which the campaign runs can differ from the chain used to reward users.
* **Usage:** From the dropdown menu, select the usage type (*MetaMorpho*, *Supply*, *Borrow*, *Collateral*) you want to incentivize. Choosing *MetaMorpho* will incentivize a MetaMorpho vault, while selecting *Supply*, *Borrow*, or *Collateral* will incentivze a Morpho Blue market.
* **Market ID:** Specify the Market ID. For *MetaMorpho*, select the desired MetaMorpho vault to incentivize. For Morpho Blue, choose whether to incentivize the *supply*, *borrow*, or *collateral* for a specific Morpho Blue market. You also have the option to manually enter the asset address for any type of Market ID.

**Restrictions:**

* **Blacklist:** Add addresses that should not receive or forward rewards. This ensures that these addresses are excluded from the campaign's rewards distribution.
* **Whitelist:** If necessary, add addresses that should be the sole recipients or forwarders of the rewards. This ensures that only these specified addresses participate in the campaign.

**Details:**

* **Deposit URL:** Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Review and Submit:**

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the *Preview Transaction* button.

<figure><img src="../../.gitbook/assets/morpho-preview-transaction.png" alt=""><figcaption></figcaption></figure>

7. **Sign and Submit**

* **Using an EOA Account:**
  * Double-check your campaign configuration.
  * Read and accept Merkl's T\&Cs by clicking on the *Accept* button and signing using your wallet.
  * Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../../.gitbook/assets/morpho-accept+approve+deposit" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Morpho Incentivization Campaign using Merkl!

{% hint style="info" %}
Please note that once created, your campaign may take up to one hour to become visible on the front-end.
{% endhint %}

* **Using a multisig wallet (Safe Wallet):**

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

{% content-ref url="../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md" %}
[deploy-your-campaign-from-a-multisig-or-gnosis-safe.md](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md)
{% endcontent-ref %}

{% hint style="info" %}
Please note that once created, your campaign may take up to one hour to become visible on the front-end.
{% endhint %}