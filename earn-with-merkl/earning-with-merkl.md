---
description: Everything you need to know to earn rewards with Merkl
---

# Earn with Merkl

{% hint style="warning" %}
**Before your start**

Make sure you understand:

* The difference between an [opportunity and a campaign](../merkl-mechanisms/technical-overview.md#opportunity-vs.-campaign)
* Key terms: [daily rewards, APR, and TVL](../merkl-mechanisms/technical-overview.md#main-metrics)
* How the [Merkl App](../merkl-mechanisms/technical-overview.md#merkl-app) is structured
{% endhint %}

All yield opportunities from protocols and chains distributing rewards through Merkl are available on the [Merkl App](https://app.merkl.xyz/).

<figure><img src="../.gitbook/assets/Capture d’écran 2025-09-17 à 10.51.55 2.png" alt=""><figcaption></figcaption></figure>



## How to get started

#### 1. Explore opportunities

Visit the [Merkl App](https://app.merkl.xyz/) and browse the available opportunities to find the one that suits you best. You can filter by chain, protocol, category, status, and more for an easy overview.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-18 à 17.55.17 1.png" alt=""><figcaption></figcaption></figure>

#### 2. Check campaign details

Select an opportunity and review campaign details such as APR, TVL, daily rewards, end date, and distribution strategy.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-18 à 17.56.35 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Make sure to check the **eligibility rules** (e.g., blacklist, health factor) to confirm you qualify for rewards.
{% endhint %}

{% hint style="info" %}
Do not get lured by high APRs. Rewards may be limited to whitelisted addresses or mostly go to large positions.
{% endhint %}

#### 3. Interact with the protocol

Use the CTA button on the opportunity page to interact directly with the incentivized protocol (e.g., supply, lend, borrow).

{% hint style="danger" %}
Always do your own research (DYOR) before investing and ensure you trust the protocol you’re interacting with.

**Merkl is non-custodial**. No funds are held in Merkl smart contracts. You interact directly with the protocols, while Merkl tracks your onchain activity and distributes rewards accordingly.

The main risks when using Merkl come from the underlying protocols, which may have smart contract vulnerabilities or operational issues that could put your funds at risk.

Merkl is not responsible for any issues arising from incentivized protocols running campaigns on the platform.
{% endhint %}

#### 4. Start earning rewards

Once you’ve interacted with the protocol, you’ll start earning rewards automatically. No staking or additional onchain actions are required.

#### 5. Claim your rewards

Visit your [Merkl dashboard](https://app.merkl.xyz/users/) to track and claim your rewards. Ensure you are connected with the same wallet used to interact with the protocol. Make sure to claim all your rewards within a year of receiving them!

<figure><img src="../.gitbook/assets/Capture d’écran 2025-11-12 à 15.14.56 1.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
Rewards usually show up in your dashboard around 8 hours after you interact with the protocol, giving Merkl time to calculate and distribute them. Check the [Status page](https://app.merkl.xyz/status) for details on reward distribution per chain.
{% endhint %}

{% hint style="info" %}
You can claim all your rewards per chain at once to optimize gas costs!
{% endhint %}

## Advanced features

Rewards on Merkl do not increase block by block, but can be claimed at a frequency which depends on the chain. You can check the claim frequency on the [Status page](https://app.merkl.xyz/status).

Note that, by default, rewards can only be claimed by the address that earned them. You can however approve an operator to claim on your behalf by calling the function `toggleOperator` on the [distributor smart contract](https://app.merkl.xyz/status). However, rewards will still be sent to the original address that earned them.

So to sum up, assuming Alice earned the rewards:

* by default only Alice can claim and rewards are sent to Alice.
* by calling `toggleOperator`, Alice can allow Bob to claim on her behalf. Then, Bob can claim for Alice by sending Alice's proof to the contract, and rewards are then sent to Alice.

If you can't call `toggleOperator` and are stuck, please [open a tech ticket in our Discord ](https://discord.com/channels/1209830388726243369/1210212731047776357), the team may be able to call it on your behalf.

### Claiming from a multisig

When claiming rewards from a multisig address, we recommend delegating the claim process to an operator. This is because Merkl reward proofs are only valid for on average four hours (varies depending on the chain). If you fail to gather the necessary signatures and execute the claim transaction before a Merkle root is updated, your claim transaction will fail.

### Claiming as an operator

Once an address has approved you as an operator by calling `toggleOperator` on the [distributor smart contract](https://app.merkl.xyz/status), you can claim rewards on behalf of that address directly from the Merkl frontend.

#### How to claim via the frontend

1. **Navigate to the user's dashboard**: Go to `https://app.merkl.xyz/users/[ADDRESS]` where `[ADDRESS]` is the wallet address for which you want to claim rewards.

2. **Enable Operator mode**: On the right side of the dashboard, you'll find a button with three vertical dots (⋮). Click on it and select **"Operator mode (advanced)"** to enable it.

   {% hint style="warning" %}
   **Operator mode (advanced)**
   
   Claim rewards on behalf of addresses that approved you. Claims without approval will fail.
   {% endhint %}

3. **Connect your operator wallet**: Connect the wallet that has been toggled as an operator for the address you're claiming on behalf of.

4. **Claim the rewards**: Once connected, you'll see the rewards available for the address, and you can claim them as you would normally. The rewards will be sent to the original address that earned them, not to your operator address.

{% hint style="info" %}
If you need to claim rewards programmatically or via the API instead of the frontend, you can retrieve the Merkle proof for a specific user and chain by calling: `https://api.merkl.xyz/v4/users/[ADDRESS]/rewards?chainId=[CHAIN_ID]`

This response will include the Merkle proof data needed to submit a claim. You can then use this information to call the claim function on the [Merkl Distributor contract](../integrate-merkl/smart-contract-addresses.md) via a block explorer or directly onchain.

To structure the claim:

* `users`: Provide the address(es) you're claiming on behalf of. If claiming for multiple tokens, repeat the user address for each token.
* `tokens`: List the token addresses you're claiming.
* `amounts`: Use the amount field from the Merkl API response for each token.
* `proofs`: Include the Merkle proof array for each token, also retrieved from the API.

<figure><img src=".gitbook/assets/DistributorClaim.png" alt=""><figcaption><p>Claiming Merkl rewards using a block explorer</p></figcaption></figure>
{% endhint %}

### Address Remapping

If a smart contract you use can’t claim rewards, ask the Merkl team to remap those rewards to a claimable wallet by reaching out on [Discord](https://discord.com/channels/1209830388726243369/1210212731047776357) with the campaign ID, source address, and destination address. The full walkthrough lives in the [Reward Forwarding guide](../merkl-mechanisms/reward-forwarding.md#address-remapping).
