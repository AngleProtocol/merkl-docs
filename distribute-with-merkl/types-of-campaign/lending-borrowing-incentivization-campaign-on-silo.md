---
description: Guide to create a Silo Campaign
---

# Lending/Borrowing Incentivization Campaign on Silo

Creating a Silo campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**:
   * Go to the Merkl's App and go to the campaign creation section by selecting "Create Campaign" from the dashboard.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the _Create Campaign_, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. **Connect your Wallet**

**Connect your wallet and select the chain on which you want to distribute the rewards.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your Silo Campaign**

* Once your token is whitelisted, you can create your Silo campaign by clicking on the 'Lend/Borrow on Silo' button on the campaign creation page (see screenshot below).

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to this page (see screenshot above), where you will need to fill out the following information:

**Distribution Details**:

* **Total Rewards**: Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.&#x20;
* **Duration**: Set the start and end dates for the campaign.
* **Min Rewards/Hour**: Ensure your distribution per hour is above the minimum rewards per hour. The Min Rewards/Hour is set at $1 per hour.

**Silo Asset Selection**:

* **Chain**: Choose the blockchain network (Ethereum, Arbitrum, Optimism, etc.) where the campaign will be conducted. Note that the chain on which the campaign runs can be different from the chain on which you reward users.
* **Usage**: Select the specific usage for your Silo campaign (Deposit, Protected Deposit, or Debt)
* **Version**: Choose the version of Silo (Silo Legacy, or Silo Llama)
* **Asset**: Select the asset to incentivize from the dropdown menu or enter the asset address manually.

**Markets Configuration**:

* Enable the markets where depositing or borrowing the asset will be incentivized. **Ideally, only one market should be enabled.**

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

**Restrictions:**

* **Blacklist:** Automatically blacklist silos that should not receive or forward rewards by properly calibrating your markets in market configuration. This ensures that non-eligible silos are excluded from rewards distribution.
* **Whitelist:** Automatically whitelist a silo if the asset should be incentivized only for a specific market. Proper market calibration will fill this field accordingly.

In the example (see screenshot above), only the PT-rsETH-26SEPT2024 market is enabled, meaning this market is whitelisted, while all others are blacklisted.

**Further Restrictions:**&#x20;

* If you want to whitelist or blacklist EOAs (Externally Owned Accounts), you can do so manually by adding them.

**Details**:

* **Deposit URL**: Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Review and Submit**

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the "Preview Transaction" button.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

7. **Sign and Submit**

* **Using an EOA Account**:
  * Double-check your campaign configuration.
  * Read and accept Merkl's T\&Cs by clicking on the "Accept" button and signing using your wallet.
  * Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Silo Incentivization Campaign using Merkl!

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_

* **Using a multisig wallet (Safe Wallet):**&#x20;

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.&#x20;

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

{% content-ref url="../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md" %}
[deploy-your-campaign-from-a-multisig-or-gnosis-safe.md](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md)
{% endcontent-ref %}

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_
