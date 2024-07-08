---
description: Guide to create an ERC20 Campaign (Token, LP Token, Lending/Borrowing Token)
---

# ERC20 Incentivization Campaign

ERC20 campaigns incorporate various aspects such as token balance, LP token balance (V2 liquidity pools), and lending and borrowing protocols. Since all these protocols are based on ERC20 tokens, Merkl can integrate them by default, ensuring that users participating in different activities can earn rewards.&#x20;

However, for V2 AMMs and lending and borrowing protocols, **we strongly recommend you to be fully supported by Merkl**. The advantages of being supported by Merkl include APR and TVL calculations, among other benefits. Additionally, you can create protocol-specific or campaign-specific types of campaigns for more tailored incentives.

The current supported lending and borrowing protocols by Merkl are:

* Silo (protocol-specific campaigns)
* Radiant (protocol-specific campaigns)
* Compound
* Gearbox
* Ionic
* Sturdy
* Metamorpho Lending

Merkl also currently support the following V2 AMMs:

* Uniswap V2
* Balancer V2
* Velodrome V2
* Aerodrome V2
* Fenix V2
* Poolside V2
* Quickswap V2

if you want your AMM V2, or lending and borrowing protocol to be fully integrated and supported by Merkl, or to create protocol or campaign-specific campaigns (as we did for Silo and Radiant), please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process. Integration allows APRs and TVL calculations.&#x20;

## Creating an ERC20 Campaigns

Creating an ERC20 campaign on Merkl is a straightforward process. Follow these steps to set up and launch your campaign effectively:

**Step-by-Step Process:**

1. **Access the Campaign Creation Page**:
   * Go to the Merkl's App and go to the campaign creation section by selecting "Create Campaign" from the dashboard.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

2. **Whitelist your Token**

* After clicking on the _Create Campaign_, this will redirect you to the page below. But first we need to whitelist your token. Fill up the following [form](https://tally.so/r/3y2bqx) - you can also access by clicking where the red square is (see screenshot) on the Merkl App.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

3. **Connect your Wallet**&#x20;

* **Connect you wallet and select the chain on which you want to distribute the rewards.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations). **One of the core features of Merkl is the ability to incentivize activity on one chain while distributing rewards on another.**

4. **Create your ERC20 Campaign**

* Once your token is whitelisted, you can create your ERC20 campaign by clicking on the 'Token Balance' button on the campaign creation page (see screenshot below).

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

* You will then be redirected to this page (see screenshot above), where you will need to fill out the following information:

**Distribution Details**:

* **Total Rewards**: Enter the total amount of rewards to be distributed. Keep in mind that a 3% maintenance fee is applied.&#x20;
* **Duration**: Set the start and end dates for the campaign.
* **Min Rewards/Hour**: Ensure your distribution per hour is above the minimum rewards per hour. The Min Rewards/Hour is set at $1 per hour.

**Target:**

* **Chain**: Choose the blockchain network (Ethereum, Arbitrum, Optimism, etc.) where the campaign will be conducted. Note that the chain on which the campaign runs can be different from the chain on which you reward users.
* **Token Address**: Select the address of the token (it can be a simple token, or the address of the LP token, or the borrowing or lending token) you want to incentivize by pasting it in the token address field.&#x20;

**Restrictions:**

* **Blacklist**: Add addresses that should neither receive nor forward rewards. **Make sure to blacklist contracts you own and therefore hold large amounts of the incentivized token.** These could be addresses that are not capable of claiming rewards or that you don't want to reward because you aim to incentivize other users with your token, LP token, or lending/borrowing token.
* **Whitelist**: If necessary, whitelist addresses to ensure that only these addresses received the rewards.

**Staking contracts:**

Merkl supports a staking mechanism where users can earn rewards even if the incentivized asset isn't directly present in their wallet.\
\
For example, users who stake their USDa will receive stUSD and even with this exchange , if you want to incentivize holders of USTa, they can still earn rewards. This works though a forwarder and this is what you need to fill if you have the need to. The forwarder includes users who have staked their USDa, ensuring they receive rewards despite not having the original tokens in their wallets since their USDa are locked in the stUSD smart contracts.\
\
**If the token you are incentivizing can be staked in another contract (such as staking USDa in the stUSD contract), Merkl can trace back the liquidity in the staking contract to the original user.** For this to work, you need to provide the staking contract addresses below. The contract where users stake their tokens is the recipient of the initial rewards. The token issued when staking the token is the token to forward rewards to, and this contract needs to be an ERC20 token. Most of the time, these are the same contracts, so you should enter the same address twice.

* **Recipient of the Initial Rewards:** The contract where users initially stake their tokens.
* **Token to Forward Rewards To:** The ERC20 token contract that issues tokens when staking and to which the rewards will be forwarded to the users holding such staked tokens.&#x20;

For more details on ERC20 Forwarders check out this [page](../../merkl-mechanisms/architecture-and-technical-overview/erc20-mechanisms.md).

**Details**:

* **Deposit URL**: Provide the deposit URL where users can participate in the campaign. This URL should direct them to the relevant page for participating in the campaign.

6. **Preview Transaction**

* Double-check all the information entered for accuracy. Once you've finished configuring your campaign, proceed by pressing the "Preview Transaction" button.

<figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

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
