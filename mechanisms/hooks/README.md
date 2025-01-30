# 🪝 Customazibility Hooks

Merkl allows incentive providers to customize campaign behavior using hooks, adding flexibility beyond standard campaign parameters.

Below are some of the most commonly used hooks in Merkl.

## 🔁 Forwarders

### Merkl Forwarders

Merkl Engine can smartly enable users to accrue rewards even if the incentivized asset isn't directly present in their wallet. Take the case of a campaign incentivizing holders of the token, but many of these tokens are staked in a staking contract: if the Merkl engine has integrated the staking contract, it can reward users which have provided liquidity on the staking contract based on how much they have staked.

This functionality to track user activity across various protocols and contracts is called Merkl `forwarders`. [Each campaign type](mechanisms/README.md) comes with its own forwarders.

Forwarders enable reward distribution to users who hold an incentivized asset indirectly (e.g., staked tokens, LP tokens).

**How It Works**:

- Forwarder integration varies by complexity. Simple forwarders, such as staking contracts for ERC20 tokens, are automatically supported by Merkl. However, more complex forwarders,like Active Liquidity Management (ALM) protocols for concentrated liquidity, or non-standard staking contracts, require a dedicated integration within Merkl.
- Some campaign types (e.g., Concentrated Liquidity Campaigns) automatically detect integrated forwarders, requiring no manual setup when creating a campaign
- For ERC20 or lending/borrowing campaigns, incentive providers can specify ERC20 forwarders or integrated forwarders that commonly hold the incentivized token.

**Example: Staked Token Rewards**:

- A campaign incentivizes USDA holders.
- Users who staked USDA and received stUSD would normally be ineligible for rewards because they don't hold the USDA directly in their wallet.
- With forwarding enabled, Merkl recognizes stUSD holders as USDA holders and distributes rewards accordingly.

**How to Enable Forwarding**:

When creating a campaign:

- Provide the staking contract address (recipient of the rewards).
- Provide the forwarded token address (token users receive when staking).

{% hint style="info" %}
Most of the time, these are the same contract address, so you may need to enter the same address twice when setting up a campaign.
{% endhint %}

For the non-standard forwarders that are not automatically detected (e.g everything but ALMs in concentrated liquidity campaigns), you may also directly specify during campaign creation.

## ✅ Whitelisting

Whitelisting restricts rewards to a specific address or set of addresses (e.g., selected forwarders, ALMs, or individual users).

**Example: Uniswap V3 ALM Incentives**:

- A campaign incentivizes LPs in a Uniswap V3 pool with three Automated Liquidity Managers (ALMs).
- The incentive provider whitelists only two ALMs and one user address.
- Result:

      - Only liquidity providers using the two approved ALMs or the whitelisted user will receive rewards
      - Rewards are distributed normally among whitelisted addresses based on liquidity share.

**Whitelisting & Blacklisting Priority**:

- Whitelisting overrides blacklisting: If an address is whitelisted, all other addresses are automatically blacklisted.
- If multiple campaigns run on the same pool, some may have whitelists while others do not.
- Users should check the campaign details to confirm eligibility requirements.

## ❌ Blacklisting

Blacklisting excludes specific addresses from receiving rewards.

- If a forwarder is blacklisted, all associated users are also ineligible.
- Example: Staking Contract Blacklist
  - A user holds 10 USDA but has staked 5 USDA in a blacklisted staking contract.
  - Only the 5 USDA in the user’s wallet qualifies for rewards.
  - Impact: Blacklisted addresses cannot earn rewards. The campaign rewards are distributed among eligible participants.

## 🚫 OFAC Compliance

Merkl provides an automatic OFAC sanctions blacklist hook, ensuring that any addresses flagged by the Office of Foreign Assets Control (OFAC) are excluded from all campaigns.

- The blacklist is continuously updated.
- Helps protocols comply with international sanctions.

## 🌉 Incentivized bridged liquidity

Merkl has partnered with Jumper to allow incentive providers to reward users for bridging liquidity from another chain.

**Why Use This?**:

- Instead of rewarding liquidity movement within the same chain, incentives can target cross-chain liquidity inflows.
- Helps chains attract new liquidity from external networks.

**How It Works**:

- Only users who bridged funds via a whitelisted bridge will be eligible.
- Ensures that only genuine cross-chain liquidity movement is rewarded.

## 🗯️ Boosting Rewards with Merkl

Merkl allows incentive providers to boost rewards for users holding a specific token or NFT.

- Similar to Curve’s vote-escrowed boost formula but with more flexibility.
- No 2.5x limit – You can customize boost multipliers as needed.

### Boost Formula Computation

$$
B = b \times \frac{R \times v}{V \times r} + 1
$$

Where:

- **B**: Boost multiplier
- **b**: Custom boost factor chosen by incentive provider
- **R**: Total rewards per epoch
- **r**: User’s reward per epoch
- **V**: Total supply of the boost token/NFT
- **v**: User’s holdings of the boost token/NFT

### NFT-Based Boosting

- Boosts can be based on NFT holdings.
- Contact us for help setting up NFT-based reward boosts.

## 🎯 Eligibility Filters

Merkl allows eligibility requirements based on:

- Minimum token holdings
- Holding duration

Example:

- A campaign requires 500 stUSD held for 30 days.
- A user holding 600 stUSD for 44 days qualifies.
- A user holding 400 stUSD for 60 days does not qualify.

This hook allows incentive providers to reward only addresses that meet both a minimum token holding requirement and a set duration. For example, if the token chosen for eligibility is stUSD, and the threshold is set to 500 stUSD with a duration of 30 days, a user holding 600 stUSD for 44 days would qualify for rewards.

Addresses that fail to meet either the token amount or duration threshold are excluded from the campaign’s rewards, ensuring incentives are focused on long-term participants rather than short-term holders.

## 🔄 Dynamic Boosting – API Hook (Coming Soon)

## 🎟️ Raffles (Coming Soon)
