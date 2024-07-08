---
description: Guide to create a Radiant Campaign
---

# Lending/Borrowing Incentivization Campaign on Radiant

Creating a Radiant campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**:
   * Go to the Merkl's App and go to the campaign creation section by selecting "Create Campaign" from the dashboard.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the _Create Campaign_, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. **Connect you Wallet**

**Connect your wallet and select the chain on which you want to distribute the rewards.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your Radiant Campaign**

* Once your token is whitelisted, you can create your Radiant campaign by clicking on the "_Lend/Borrow on Radiant"_ button on the campaign creation page (see screenshot below)

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to this page (see screenshot above), where you will need to fill out the following information:

**Distribution Details**:

* **Total Rewards**: Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.&#x20;
* **Duration**: Set the start and end dates for the campaign.
* **Min Rewards/Hour**: Be sure that your distribution per hour is superior than the Min Rewards/Hour. The Min Rewards/Hour is set at $1 per hour.

**Radiant's token to target:**

* **Chain**: Choose the blockchain network (Ethereum, Arbitrum, Optimism, etc.) where the campaign will be conducted. Note that the chain on which the campaign runs can be different from the chain on which you reward users.
* **Asset**: Select the asset to incentivize from the dropdown menu or enter the asset address manually.
* **Cap:** Set the cap (in USDC). This is used by radiant; if you're not the radiant protocol you should set it to 0.&#x20;

**Restrictions:**

* **Blacklist:** Add addresses that should not receive or forward rewards. This ensures that these addresses are excluded from the campaign's rewards distribution.
* **Whitelist:** If necessary, add addresses that should be the sole recipients or forwarders of the rewards. This ensures that only these specified addresses participate in the campaign.

**Details**:

* **Deposit URL**: Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Review and Submit**:

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the "Preview Transaction" button.

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

7. **Sign and Submit**

* **Using an EOA Account**:
  * Double-check your campaign configuration.
  * Read and accept Merkl's T\&Cs by clicking on the "Accept" button and signing using your wallet.
  * Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Radiant Incentivization Campaign using Merkl!

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_

* **Using a multisig wallet (Safe Wallet):**&#x20;

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.&#x20;

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

{% content-ref url="../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md" %}
[deploy-your-campaign-from-a-multisig-or-gnosis-safe.md](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md)
{% endcontent-ref %}

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_
