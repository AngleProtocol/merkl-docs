---
description: >-
  You can launch an incentive campaign on your own in just a few minutes using
  Merkl Studio. However, there are a few prerequisites to complete beforehand.
---

# Before you start

## ⚙️ Merkl’s core principles

Before setting up your campaign on Merkl, make sure you have read and understood:

* [**Merkl’s reward distribution process**](../merkl-mechanisms/technical-overview.md) (reward computation vs. reward update, dispute period, etc.)
* [**The different incentive mechanisms**](../merkl-mechanisms/incentive-mechanisms.md) (campaign types, distribution types, scoring types)
* How to personalize your campaign using [**customization options**](../merkl-mechanisms/customization-options.md)
* [**Merkl forwarding system**](../merkl-mechanisms/reward-forwarding.md)
* [**Additional features**](../merkl-mechanisms/features.md) that can make your campaign unique

{% hint style="info" %}
If you're looking for some incentive mechanism for which you're not sure about whether it's supported, or looking to add incentive features or customization options that are not displayed on the platform, please contact us!
{% endhint %}

Some Merkl features (e.g., new distribution types, campaign types, customization options) may not be directly available in the campaign creation flow. If a required configuration option is not available in the frontend, we can:

* Help you generate the correct campaign payload.
* Provide dedicated API endpoints so you can structure your campaign as needed.

## 📝 Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted.

We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users. **Campaigns must distribute at least ~$1/hour in total rewards to avoid dusting.**

You can check whether your token is already whitelisted by setting it as the reward token in the [Merkl studio](https://studio.merkl.xyz/create-campaign/clamm). If your token is not whitelisted, please submit the reward token’s price source and logo by filling out this [whitelisting form](https://anglemoney.notion.site/1aecfed0d48c808a8194fe2befd50bdb?pvs=105).

{% hint style="success" %}
Make sure you have all the tokens you want to distribute in your wallet when creating a campaign
{% endhint %}

## 💳 Token Predeposit System

Merkl offers a **predeposit system** that allows campaign creators to pre-fund their campaign tokens directly into the `DistributionCreator` contract. This system provides several advantages:

* **Gas optimization**: Pre-fund tokens once and use them for multiple campaigns without repeated approvals
* **Better token management**: Centralize your campaign tokens in one place
* **Operator delegation**: Enable operators to manage campaigns on your behalf using predeposited balances

### How it works

The predeposit system uses two key mechanisms:

1. **Creator Balance** (`creatorBalance`): Stores tokens predeposited by each campaign creator
2. **Creator Allowance** (`creatorAllowance`): Allows creators to grant spending permissions to operators

{% hint style="info" %}
**Finding the DistributionCreator contract address**: You can find the `DistributionCreator` contract address for your chain on the [Merkl Status page](https://app.merkl.xyz/status). Select your chain and click on **Creator** to view the contract address and access it directly in your chain's block explorer.
{% endhint %}

### How to deposit tokens

The most common workflow is for a Safe (multisig) or individual wallet to deposit tokens for themselves and then grant permissions to operators. Here's how it works:

#### Step 1: Approve the contract

First, approve the `DistributionCreator` contract to spend your tokens:

```solidity
IERC20(token).approve(distributionCreatorAddress, amount)
```

{% hint style="tip" %}
**Tip**: You can approve `type(uint256).max` for unlimited approval if you plan to deposit multiple times—this is more gas efficient.
{% endhint %}

#### Step 2: Deposit tokens into your creator balance

Call `increaseTokenBalance()` to transfer tokens from your wallet into your creator balance:

```solidity
increaseTokenBalance(yourAddress, rewardToken, amount)
```

**Parameters:**

* `yourAddress`: the Safe or wallet address
* `rewardToken`: The token address you want to deposit 
* `amount`: The amount of tokens to deposit

**Example:**

```solidity
// Your Safe wants to deposit 10,000 aglaMerkl
IERC20(aglaMerkl).approve(distributionCreator, type(uint256).max);
distributionCreator.increaseTokenBalance(safeAddress, aglaMerkl, 10000e18);
// ✅ 10,000 aglaMerkl transferred from Safe wallet
// ✅ creatorBalance[safeAddress][aglaMerkl] = 10000e18
```

#### Withdrawing tokens

To withdraw tokens from your creator balance back to your wallet:

```solidity
decreaseTokenBalance(yourAddress, rewardToken, recipientAddress, amount)
```

**Example:**

```solidity
// Withdraw 5,000 aglaMerkl from your creator balance to your Safe
distributionCreator.decreaseTokenBalance(safeAddress, aglaMerkl, safeAddress, 5000e18);
// ✅ creatorBalance[safeAddress][aglaMerkl] decreased by 5000e18
// ✅ 5,000 aglaMerkl transferred back to Safe wallet
```

#### Depositing for others

Regular users can deposit tokens on behalf of other addresses, but the tokens will always come from the caller's wallet:

```solidity
// Alice deposits tokens but credits Bob's balance
distributionCreator.increaseTokenBalance(Bob, aglaMerkl, 1000e18);
// ⚠️ Tokens are taken from Alice's wallet, not Bob's
// ✅ creatorBalance[Bob][aglaMerkl] = 1000e18
```

**Note**: Once tokens are credited to Bob's balance, Bob becomes the owner and only Bob (or a governor) can withdraw them.

#### Step 3: Grant allowances to operators

Once you have tokens in your creator balance, you can grant spending permissions to operators:

```solidity
increaseTokenAllowance(yourAddress, operator, rewardToken, amount)
```

**Parameters:**

* `yourAddress`: Your address (the owner of the balance)
* `operator`: The operator address you want to authorize
* `rewardToken`: The token for which to grant allowance
* `amount`: Maximum amount the operator can spend from your balance

**Example:**

```solidity
// Grant operator permission to spend up to 5,000 aglaMerkl
distributionCreator.increaseTokenAllowance(safeAddress, operatorAddress, aglaMerkl, 5000e18);
// ✅ creatorAllowance[safeAddress][operatorAddress][aglaMerkl] = 5000e18
// ✅ Operator can now create campaigns using up to 5,000 aglaMerkl from your balance
```

### Setting up operators

To delegate campaign management to operators, you need to grant **two separate permissions**:

1. **Campaign management permission**: `toggleCampaignOperator(user, operator)` - Authorizes operators to create and manage campaigns (toggle on/off)
2. **Token spending allowance**: `increaseTokenAllowance(user, operator, rewardToken, amount)` - Grants operators permission to spend your predeposited tokens

Both permissions are required for an operator to fully manage campaigns using your predeposited balance. See the [Campaign Operators section](campaign-management.md#campaign-operators) for more details.

### Creating campaigns on behalf of a creator

When an operator creates a campaign on behalf of a creator, it's **critical** to set the correct `creator` parameter in the campaign parameters. The system will look for the predeposited balance and allowance based on this `creator` address.

**Operator creates campaign:**

```solidity
CampaignParameters memory campaign = CampaignParameters({
    creator: creatorAddress,  // ✅ MUST be the address that granted the allowance and has the predeposited balance!
    // ... other parameters
});

distributionCreator.createCampaign(campaign);
```

**⚠️ Common mistake:**

If the operator sets `creator = address(0)` or `creator = operatorAddress`:

* ❌ System looks for `creatorBalance[operatorAddress][token]` = 0
* ❌ Fallback → takes tokens from the operator's wallet instead
* ⚠️ If the operator doesn't have sufficient tokens in their wallet, the transaction will revert

{% hint style="warning" %}
**Important**: Always set the `creator` parameter to the address that granted the allowance and has the predeposited balance. Otherwise, the system will fall back to the operator's wallet, and the transaction will revert if the operator has insufficient tokens.
{% endhint %}

**⚠️ Insufficient predeposited balance or allowance:**

If the operator correctly sets the `creator` parameter but encounters one of these issues:

* **Insufficient predeposited balance**: The creator's `creatorBalance[creatorAddress][token]` is insufficient or zero
* **Allowance exceeded**: The operator's allowance has been exceeded or was never granted

The system will attempt a **fallback**: it tries to take tokens from the operator's wallet instead.

* ✅ If the operator has sufficient tokens in their wallet → the transaction succeeds (using the operator's tokens), and **the operator becomes the creator** of the campaign
* ❌ If the operator's wallet balance is insufficient → the transaction will **revert**

⚠️ The creator should ensure:
* Sufficient tokens are predeposited before an operator attempts to create a campaign
* The operator has been granted sufficient allowance to spend the predeposited tokens

Otherwise, the campaign creation will fall back to using the operator's wallet balance, and **the operator will become the campaign creator** instead of the intended creator address, which may not be the intended behavior.

{% hint style="info" %}
Before creating a campaign, verify that the creator has predeposited enough tokens to cover the campaign amount plus fees, and that the operator has sufficient allowance. If multiple campaigns are created in sequence, track both the remaining predeposited balance and remaining allowance to avoid unintended fallback behavior or failed transactions.
{% endhint %}

## 🧪 Test campaigns

You may want to start testing the flow and integrating our data before your point program starts. Merkl is not deployed on testnets, but you can still run test campaigns using our test token: **aglaMerkl**.

These test campaigns are not mandatory — they're available to help you experiment with how Merkl works using valueless tokens, so you can better prepare your upcoming campaigns. However, since they consume computation resources on our end, we encourage you to use this feature only when there's a clear need.

Note that test campaigns will **not appear on the Merkl app** and will not be visible to users, as they are automatically filtered out from the interface.

To get started, send us a message and we'll provide you with our test token. aglaMerkl has no value and is only intended for creating test campaigns on Merkl.

You can then create your test campaign using aglaMerkl tokens. While the campaign won't show up in the Merkl app, you'll be able to **check everything is working properly via the Merkl API**, using the campaign ID generated at creation. You can refer to the [API integration page](https://docs.merkl.xyz/integrate-merkl/app) for how to effectively use the API.

Make sure to verify that:

* distribution is functioning as expected
* key metrics like TVL are correctly computed

If everything looks good in the API, it will appear correctly in the Merkl front end once the real campaign is live!

If you need help to create your first test campaigns, feel free to reach out to us.

## 💰 What budget should I set for my campaign?

The ideal campaign budget depends on your specific objectives and the distribution method you choose. Campaign budgets follow a natural supply-and-demand dynamic: liquidity providers want to maximize their rewards, while campaign creators aim to minimize costs. As a result, TVL typically scales proportionally with the reward budget you allocate.

Depending on your campaign type, you need to account for fees differently (see our [fees section](../distribute-with-merkl/fee-model.md) for details).

**Distribution methods matter**: If you're concerned about over-distributing rewards early in your campaign, consider using a [capped APR campaign](../merkl-mechanisms/distributions.md#-capped-reward-rate-campaigns). This approach ensures you don't exhaust your budget too quickly if TVL starts low, as rewards are distributed at a fixed rate relative to TVL rather than depleting a fixed token amount.

**Benchmarking APRs**: To gauge appropriate reward levels, you can:

* Browse existing campaigns on the [Merkl app](https://app.merkl.xyz) to see typical APRs in your category
* Use Merkl's [APR simulation tool](https://docs.google.com/spreadsheets/d/1suPzoiFfZP3_XdXNu9qh63gNUbbXaWoKuhy17FRCEIc/edit?usp=sharing) to model how different reward budgets translate to APRs at various TVL levels. To use this tool, you may simply duplicate the sheet and input your own numbers to simulate.
* Adjust your budget based on competitive rates and your growth targets

{% hint style="success" %}
**Start small, iterate often**: We recommend launching shorter campaigns initially (e.g., 2 weeks) to test performance and gather data. You can then create follow-up campaigns with optimized parameters based on actual TVL, user engagement, and APR effectiveness. This iterative approach minimizes risk and maximizes ROI.
{% endhint %}

## 📃 Merkl terms

You must also make sure that you have read and understood [Merkl's Terms & Conditions](https://app.merkl.xyz/terms). You will be required to agree to these terms during the campaign setup process.