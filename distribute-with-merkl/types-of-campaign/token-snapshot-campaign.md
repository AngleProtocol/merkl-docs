---
description: Guide to set a Token Snapshot Campaign
---

# Token Snapshot Campaign

Creating an Airdrop Campaign based on Concentrated Liquidity (CLAMM) Pool Incentive campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**:
   * Go to the Merkl's App and go to the campaign creation section by selecting "Create Campaign" from the dashboard.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the _Create Campaign_, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. **Connect your Wallet**

**Connect your wallet and select the chain on which you want to distribute the rewards.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your Token Snapshot Campaign**

* Once your token is whitelisted, you can create your Token Snapshot campaign by clicking on the appropriate button (see screenshot below).

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to this page (see screenshot above), where you will need to fill out the following information:

**Distribution Details**:

* **Total Rewards**: Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.&#x20;
* **Snapshot**: Set the time for the snapshot (note that it will be taken at the block prior to this date).
* **Distribution Date**: Set the date on which you want your rewards to be distributed.

**Target:**

* **Chain**: Choose the blockchain network (Ethereum, Arbitrum, Optimism, etc.) where the campaign will be conducted. Note that the chain on which the snapshot is taken can be different from the chain on which you reward users.
* **Token Address**: Enter the address of the token you want to incentivize by taking a token snapshot. Paste the token address into the appropriate field.

**Restrictions:**

* **Blacklist**: Add addresses that should neither receive nor forward rewards. **Make sure to blacklist contracts you own and therefore hold large amounts of the incentivized token.** These could be addresses that are not capable of claiming rewards or that you don't want to reward because you aim to incentivize other users with your token, LP token, or lending/borrowing token.
* **Whitelist**: If necessary, whitelist addresses to ensure that only these addresses received the rewards.

**Staking contracts:**

Merkl supports a staking mechanism where users can earn rewards even if the incentivized asset isn't directly present in their wallet.\
\
For example, users who stake their USDa will receive stUSD and even with this exchange , if you want to incentivize holders of USTa, they can still earn rewards. This works though a forwarder and this is what you need to fill if you have the need to. The forwarder includes users who have staked their USDa, ensuring they receive rewards despite not having the original tokens in their wallets since their USDa are locked in the stUSD smart contracts.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

**If the token you are incentivizing can be staked in another contract (such as staking USDa in the stUSD contract), Merkl can trace back the liquidity in the staking contract to the original user.** For this to work, you need to provide the staking contract addresses below. The contract where users stake their tokens is the recipient of the initial rewards. The token issued when staking the token is the token to forward rewards to, and this contract needs to be an ERC20 token. Most of the time, these are the same contracts, so you should enter the same address twice.

* **Recipient of the Initial Rewards:** The contract where users initially stake their tokens.
* **Token to Forward Rewards To:** The ERC20 token contract that issues tokens when staking and to which the rewards will be forwarded to the users holding such staked tokens.&#x20;

**Press Add:** Don't forget to press "Add" once you've set the Recipient of the _"Initial rewards"_ and _"Token to forward rewards to"_. This ensures the rewards are forwarded to the users who have staked the token you want to incentivize.

For more details on ERC20 Forwarders check out this [page](../../merkl-mechanisms/architecture-and-technical-overview/erc20-mechanisms.md).

**Details**:

* **Deposit URL**: Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Preview Transaction**

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the "Preview Transaction" button.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

7. **Sign and Submit**

You can sign and submit using either an EOA account or a multisig wallet. The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. Regardless of the method you choose, you will need to follow these steps: sign the T\&C conditions, approve the tokens for transfer, and deposit them.

* **Using an EOA Account**:
  * Double-check your campaign configuration.
  * Read and accept Merkl's T\&Cs by clicking on the "Accept" button and signing using your wallet.
  * Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Merkl ERC20 Incentivization Campaign!

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_

* **Using a multisig wallet (Safe Wallet):**&#x20;

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.&#x20;

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

{% content-ref url="../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md" %}
[deploy-your-campaign-from-a-multisig-or-gnosis-safe.md](../deploy-your-campaign-from-a-multisig-or-gnosis-safe.md)
{% endcontent-ref %}

_**Please note that once created, your campaign may take up to one hour to become visible on the front-end.**_
