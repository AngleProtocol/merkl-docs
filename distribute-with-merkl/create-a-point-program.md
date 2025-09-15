---
description: Launch your point program with Merkl
---

# Create a point program

## 🛫 Getting started

Merkl excels at two things:

* **Indexing onchain activities** across chains and protocols
* **Distributing rewards** accurately, at scale

These two pillars can work together or independently — and many protocols take advantage of that flexibility:

* **Liquidity mining programs** typically use both features: Merkl tracks user activity and distributes tokens accordingly
* **Airdrops** use Merkl's reward distribution functionality without relying on its indexing: Merkl distributes tokens based on a list of addresses
* **Point systems**, like the ones we’ll explore here, often rely solely on Merkl’s indexing power: Merkl is used to calculate how many points each user should get based on their onchain activity, but Merkl does not distribute any tokens with attached value.

{% hint style="success" %}
While Merkl is capable of handling complex reward setups like Uniswap v4 liquidity provision, it also offers robust capabilities to track onchain activities such as LP positions, lending, or protocol usage — enabling the creation of sophisticated points campaigns based on this data.
{% endhint %}

## 🔀 Two ways to set up a point program

There are two main setups for running points programs with Merkl:

* **Tokenized points** — where points are represented by non-transferable ERC20 tokens.
* **Non-tokenized points** — where Merkl is used purely for indexing.

Let’s dive into each setup!

### Tokenized points (ERC20, non-transferable)

In this setup, **points are issued as ERC20 tokens but configured to be non-transferable**. From Merkl’s perspective, this is just like running a standard token reward campaign.

**How does it work?**

1. Whitelist Merkl’s `Creator` and `Distributor` contracts so we can distribute your points
2. Create your campaigns using Merkl’s standard process: define multipliers, reward rates, and which onchain actions to track
3. Users interact onchain as usual and can **claim their non-transferable points** via their Merkl’s dashboard

**If you later choose to airdrop a token based on points, simply map your airdrop distribution to user point balances.** Just be sure to exclude Merkl’s distributor contract, which may still hold unclaimed tokens.

**With this setup, you benefit from all of Merkl’s default features, including leaderboards and public campaign visibility in the frontend.** Merkl can even assign an estimated value to your points directly within the interface, so users see estimated APR values when farming.

This is a straightforward way to leverage Merkl’s full stack while retaining control over the transferability of your points.

### Non-tokenized points (pure indexing)

In this case, points are most likely accounted for in **an offchain database that you control**. This means that Merkl cannot mint points for you, it can just run computations based on onchain activity. Here you simply need to define the logic, Merkl tracks user actions, and you use the results however you like.

**How does it work?**

#### 1. Create a campaign using test tokens to launch computation

Merkl will give you some mock tokens on a chain of your choice (like Gnosis Chain). You'll then be able to use these tokens to create campaigns that effectively track what you’re interested in rewarding with your points (e.g providing liquidity in a pool).

You can setup these campaigns to mirror your intended logic (e.g., “1 mock token per $1,000 deposited”). Note that campaigns can be created in a fully programmatic way through easy scripts.

The campaigns you’re creating with this will effectively distribute mock tokens, but these won’t be visible to your users and users do not need to claim rewards. As explained above, Merkl cannot mint points for you, rather you need to parse the results of these campaigns to know how to allocate your rewards.

#### 2. Get the results via Merkl's API

Use Merkl’s API to retrieve the list of eligible addresses as well as their relative contribution to the pool or the protocol that you are tracking. Reward amounts are expressed in mock token units.

* Retrieve all campaigns created by your address at `https://api.merkl.xyz/v4/campaigns?creatorAddress=<YOUR_ADDRESS>&test=true`\
  (e.g. [https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701\&test=true](https://api.merkl.xyz/v4/campaigns?creatorAddress=0xA9DdD91249DFdd450E81E1c56Ab60E1A62651701\&test=true))
* Get the reward breakdown in mock token units for all the campaigns you created at `https://api.merkl.xyz/v4/rewards?chainId=&campaignId=<YOUR_CAMPAIGN_ID>&test=true`\
  (e.g. [https://api.merkl.xyz/v4/rewards?chainId=100\&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c\&test=true](https://api.merkl.xyz/v4/rewards?chainId=100\&campaignId=0x83adc24c9644324beebd26e6e2a7b9ffc14ce40d1d7cde309854ef79c9485c4c\&test=true))

{% hint style="warning" %}
Please note that the rewards endpoint is paginated — be sure to fetch all pages using `&page=<NUMBER>`
{% endhint %}

#### **3. Normalize and customize**

The mock token units (points) can be renormalized to fit your specific needs.

For example, if you create one campaign that distributes 1 point per $1,000 deposited in a protocol, and another campaign that does the same for a different protocol, you can apply different multipliers as needed when allocating points to your users.

If you want to apply a 5× multiplier for the second protocol afterwards, you can simply mint 5x more points to recipients of this campaign — if address A has earned 1 mock token in reward from the second campaign, you can assign 5 points to that address.

This approach offers complete control over point allocation, allowing you to exclude certain campaigns, selectively boost rewards for specific users, or customize the system however you like.

#### 4. **Display points on your own UI**

In this setup, you are responsible for displaying points on their own UI. However, you can easily build a leaderboard or dashboard using the data from Merkl’s mock campaigns via the Merkl API as seen above.

{% hint style="success" %}
To wrapp up

* Want **fast setup** and Merkl UI? Use **tokenized points.**
* Want **full control** and custom logic? Go with **non-tokenized points.**
{% endhint %}

## 💪🏼 Enhance your program with optional features

Create a unique points program by adding [customization options](../merkl-mechanisms/customization-options.md) to your program, such as boosts, a referral system, or eligibility rules.
