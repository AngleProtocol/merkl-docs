---
description: Guide to create a CLAMM Campaign
---

# Concentrated Liquidity Pool Incentivization Campaign

Creating a Concentrated Liquidity (CLAMM) Pool Incentive campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**:
   * Go to the Merkl's App and go to the campaign creation section by selecting "Create Campaign" from the dashboard.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the _Create Campaign_, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. **Connect you Wallet**

* **Connect your wallet and select the chain on which you want to distribute the rewards.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your CLAMM Campaign**

* Once your token is whitelisted, you can create your CLAMM campaign by clicking on the appropriate button (see screenshot below).

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to the campaign configuration page, where you will need to provide the following details:

**Distribution Details**:

* **Total Rewards**: Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.&#x20;
* **Duration**: Set the start and end dates for the campaign.
* **Min Rewards/Hour**: Ensure your distribution per hour is above the minimum rewards per hour. The Min Rewards/Hour is set at $1 per hour.

**Liquidity Pool Selection**:

* **Chain**: Choose the blockchain network (Ethereum, Arbitrum, Optimism, etc.) where the campaign will be conducted. Note that the chain on which the campaign runs can be different from the chain on which you reward users.
* **Pool:** Select the liquidity pool to incentivize from the dropdown menu or enter the pool address manually.
* **Reward allocation:**
  * **Token0**: Specify the percentage of rewards allocated to holding Token0.
  * **Token1**: Specify the percentage of rewards allocated to holding Token1.
  * **Fees**: Specify the percentage of rewards allocated to Fees.
  * **Incentivize only in-range or in-and-out-range liquidity positions**: Choose the applicable option.

**Rewards Forwarders:**

* Enable the Automated Liquidity Managers (ALM) that will forward your campaign rewards to the Liquidity Providers using an ALM, and disable the ALMs you don't want to send rewards to.

<figure><img src="../../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

* As fewer ALMs are enabled, the restriction changes from blacklisting (disabled addresses) to whitelisting (enabled addresses).

<figure><img src="../../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

**Further Restrictions:**&#x20;

* If you want to whitelist or blacklist EOAs (Externally Owned Accounts), you can do so manually by adding them.

**Boost:**&#x20;

* Boost rewards based on a token balance by providing the address of the token you want to incentivize and specifying the boost multiplier. This allows you to incentivize users to hold a specific token in their wallet to earn additional rewards.

<figure><img src="../../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

**Details**:

* **Deposit URL**: Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Preview Transaction and Submit**

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the "Preview Transaction" button.

<figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

7. **Sign and Submit**:

You can sign and submit using either an EOA account or a multisig wallet. The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. Regardless of the method you choose, you will need to follow these steps: sign the T\&C conditions, approve the tokens for transfer, and deposit them.

* **Using an EOA Account**:
  * Double-check your campaign configuration.
  * Read and accept Merkl's T\&Cs by clicking on the "Accept" button and signing using your wallet.
  *   Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.



<figure><img src="../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Concentrated Liquidity Merkl Incentivization Campaign!

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_

* **Using a multisig wallet (Safe Wallet):**&#x20;

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.&#x20;

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

{% content-ref url="../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md" %}
[deploy-your-campaign-from-a-multisig-or-gnosis-safe.md](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md)
{% endcontent-ref %}

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_
