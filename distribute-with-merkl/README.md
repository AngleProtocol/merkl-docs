---
description: Everything you need to know to create campaigns on Merkl
---

# 😽 Distribute with Merkl

Merkl is a one-stop shop for running all your incentive and growth campaigns and it only takes a few clicks and minutes to get started!

Merkl campaigns can be created at the smart contract level directly or through the Merkl frontend available [here](https://app.merkl.xyz/create) which lists the supported campaign types.

Merkl supports several big campaign types, that can all be customized with various hooks or enriched with other properties:

1. Campaigns for liquidity providers in Concentrated Liquidity Pools (CLAMM) like on Uniswap V3
2. Campaigns to incentivize holders of an ERC20 token over time (ERC20 Campaigns), which can include campaigns to incentivize lenders and borrowers of lending protocols or liquidity providers in constant product liquidity pools (like Uniswap V2 or Aerodrome V2).
3. Campaigns to incentivize Lending and Borrowing on more complex protocols like Morpho, or to incentivize more tailored behavior on lending and borrowing protocols which we support notably on Silo and Radiant
4. Campaigns to airdrop tokens to a wide range of users based on a json file or on a snapshotted token balance

{% hint style="info" %}
We are constantly working on adding support for new campaign types. This list as well as the following pages may therefore not be fully up to date with what's currently supported by the Merkl engine and frontend. If you're interested in a new incentivization use case, don't hesitate to contact us by opening a [BD ticket on our Discord](https://discord.com/invite/jnYfrGxDbe)
{% endhint %}

{% hint style="info" %}
If you are looking to run a points program and want to leverage Merkl for this, please contact us so we can provide guidance on how you can easily leverage Merkl to reduce the hassle on your operational team for this.
{% endhint %}

## Getting started

### Token whitelisting

Before starting your campaign, you need to ensure that the token you want to use as reward with Merkl has been whitelisted. We usually process token whitelisting requests once a day, and it's a way for us to ensure that we will be able to properly compute APRs and that the token will be safe to use for our users.

You can find all whitelisted tokens by chain on this [page](https://app.merkl.xyz/integrations). If your token is not part of the list, please fill out the following [form](https://tally.so/r/3y2bqx).

### Understanding the Merkl engine

It's also important to understand how the Merkl engine works. The engine calculates and distributes rewards based on the parameters you set for incentivizing liquidity providers.

The Merkl engine runs at different frequencies depending on the chain, typically every 4 to 12 hours (with an average of 8 hours). You can check the status for each chain in our app [here](https://app.merkl.xyz/status).

For example, if you create a 14-day campaign with a budget of $42,000, and the engine runs every 8 hours (3 times per day), there will be a total of 42 runs (14 days × 3 runs per day). As a result, the rewards per run would be $1,000 ($42,000 ÷ 42), i.e farmers will be able to claim $1,000 from the rewards you allocated at each engine run.

{% hint style="info" %}
For more details on the Merkl system, you can check [this page](../mechanisms/technical-overview.md) of our docs.
{% endhint %}

### Understanding the specificities of your campaign type

It's key to understand how the specific campaign you'll be running works. This [section of the docs](../mechanisms/types-of-campaign.md) details some of the specificities associated with all the specific Merkl campaigns.

Typically, if you create a concentrated liquidity campaign, you'll need to understand what the different parameters for concentrated liquidity pools can refer to.

Again, if you're looking for some incentive mechanism for which you're not sure about whether it's supported or looking to add incentive features or hooks that are not displayed on the platform, please contact us!

## Step-by-step process

1. **Access the Campaign Creation Page**
   Go to the Merkl's App and go to the campaign creation section by selecting _Create Campaign_ from the dashboard.

<figure><img src="../.gitbook/assets/create-campaign-screenshot.png" alt=""><figcaption></figcaption></figure>

2. **Verify if your token is whitelisted**

After clicking on the _Create Campaign_, this will redirect you to the page below. If not already whitelisted (you can check [here](https://app.merkl.xyz/integrations) whitelisted tokens on a given chain), fill up the whitelisting form

<figure><img src="../.gitbook/assets/whitelist-token-screenshot.png" alt=""><figcaption></figcaption></figure>

3. **Connect you Wallet**

Connect your wallet and select the chain on which you want to distribute the rewards. **The chain connected to your wallet during the campaign creation will determine where the rewards are distributed, but not necessarily where you'll be incentivizing activity.** To see all the chains that Merkl supports, check this [page](https://app.merkl.xyz/integrations).

4. **Select your campaign type**

Once your token is whitelisted, you can choose your specific campaign type by clicking on the appropriate button (see screenshot below).

<figure><img src="../.gitbook/assets/CLAMM-campaign-create-screenshot.png" alt=""><figcaption></figcaption></figure>

5. **Fill Out Campaign Details**

<figure><img src="../.gitbook/assets/CLAMM-fill-out-campaign-detail.png" alt=""><figcaption></figcaption></figure>

You will then be redirected to the campaign configuration page. The details you will need to provide depend on the campaign type you have chosen.

In many campaigns, you will be required to input a rewards amount as well as start and end dates for the campaign. The total rewards amount is the total amount of rewards to be distributed over the whole campaign duration (on top of which a maintenance fee may be applied).

For each whitelisted token, Merkl sets a minimum amount of token that can be distributed per hour. If your token amount is too low (generally parameters are calibered so you cannot distribute less than \$1 per hour), then you will not be able to create your campaign.

{% hint style="info" %}
For some campaigns, you might be wondering why there are so many parameters. The reason is simple: we want to empower you to incentivize the exact behavior you desire. This flexibility is at the core of Merkl—enabling anyone to incentivize the onchain behavior they want!
{% endhint %}

6. **Customize your campaign with hooks**

Campaigns on Merkl are fully customizable and you may typically have to choose whether you want to change from the default behavior for the campaign.
Some of the hooks supported on Merkl are detailed [here](../mechanisms/hooks/README.md). You may typically choose to:

- boost rewards for holders of a given token (following the Curve formula or your own formula)
- whitelist eligible addresses for rewards
- blacklist addresses from receiving rewards. Usually incentive providers tend to blacklist contracts that they own and can control large amount of the incentivized tokens, or addresses that are not capable of claiming rewards or that they simply don't want to reward
- only allow people who used a specific bridge to get on a chain to receive rewards
- enable users who put liquidity in some specific smart contracts to receive rewards even if they're not directly involved with the pool or the token incentivized. This is typically useful if liquidity can be staked in other smart contracts. Within this hook, you'll be asked to specify the recipient of the initial rewards (that is to say the contract where users initially staked their tokens) and the token to forward rewards to (that is to say the ERC20 token contract that issues tokens when staking and to which the rewards will be forwarded to the users holding such staked tokens). Most of the time it'll be the same contract and you'll need to input the same address twice
- And more!

Some hooks are also campaign specific. In concentrated liquidity campaigns for instance, you have the choice to leave the opportunity for out of range positions to still accrue rewards.

While the Merkl frontend usually detects the protocols incentivized and will automatically link to the right frontend associated to the opportunity, campaign creators may provide onchain the deposit URL where users can participate in the campaign.

7. **Preview Transaction and Submit**

Double-check all the information entered for accuracy. Once you have finished configuring your campaign, proceed by pressing the _Preview Transaction_ button.

<figure><img src="../.gitbook/assets/CLAMM-preview-transaction.png" alt=""><figcaption></figcaption></figure>

8. **Sign and Submit**

You can sign and submit using either an EOA account or a multisig wallet. The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder. Regardless of the method you choose, you will need to follow these steps: accept the T\&C, approve the tokens for transfer, and deposit them.

- **Using an EOA Account:**
  - Double-check your campaign configuration.
  - Read and accept Merkl's T\&Cs by clicking on the _Accept_ button and signing using your wallet.
  - Approve the tokens for transfer and deposit the amount you want to incentivize, plus the maintenance fee of 3%.

<figure><img src="../.gitbook/assets/CLAMM-accept-approve-deposit.png" alt=""><figcaption></figcaption></figure>

After these steps, congratulations! You have created your Concentrated Liquidity Merkl Incentivization Campaign!

{% hint style="info" %}
Please note that once created, your campaign may take up to one hour to become visible on the front-end.
{% endhint %}

- **Using a multisig wallet (Safe Wallet):**

The recommended method for distributing rewards with Merkl using a multisig is through the Gnosis Safe Transaction Builder.

To learn how to deploy your campaign from a multisig or Gnosis Safe Transaction Builder, check this [page](./deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) where everything is explained in more detail.

## Campaign specific details

Depending on your campaign type, you may be asked to provide specific information. While [the page associated to each campaign in the doc](../mechanisms/types-of-campaign.md) should explain in details what each parameter required is above, here are more details that you may need when creating specific campaigns.

### Airdrops

Merkl makes airdropping tokens to your community simple and efficient!
To run an airdrop with Merkl, you need to come with a JSON file essentially giving the breakdown of user addresses and the amount of rewards they need to receive.

#### JSON Format

```
type AirdropJSON = {
    // Token being distributed
    rewardToken: string;
    // User rewards
    rewards: {
        // recipient: user address
        [recipient: string]: {
            // Reason: how the user got the rewards
            [reason: string]: string; // value: amount with all decimals as a string
        };
    };
};
```

Example:

```
{
  "rewardToken": "0x1a7e4e63778B4f12a199C062f3eFdD288afCBce8",
  "rewards": {
    "0x529619a10129396a2F642cae32099C1eA7FA2834": {
      "reason1": "1827034243855622331"
    },
    "0x0C2553e4B9dFA9f83b1A6D3EAB96c4bAaB42d430": {
      "reason1": "1827034243855622331",
      "reason2": "1035298980258165611"
    }
  }
}
```

- **rewardToken:** The token being distributed.
- **rewards:** User rewards with recipient addresses and reasons for the rewards.

#### Important Note

Merkl applies a fee of 0.5%. Therefore, if the total amount of tokens to be airdropped in your JSON file sums up to 100,000, then 100,000 will be distributed to the recipients, and a fee will be applied on top of it. To ensure that 100,000 tokens reach your users, you need to provide 100,502.512563 tokens (100,000 / (1 - 0.5%)).

Our frontend will automatically calculate the correct amount for you.

If you prefer to have exactly 100,000 tokens leave your wallet, then the sum of the amounts in your JSON file should be 99,500 tokens (100,000 \* (1 - 0.5%)).

#### Token Snapshot

Merkl also leaves you the possibility to run an airdrop based on token balance at a chosen moment in time. This is what is enabled by Token Snapshot campaigns.
For this type of campaign, beyond the rewards amount, you'll be asked to input the following parameters

- **Snapshot:** Set the time for the snapshot - note that it will be taken at the block prior to this date.
- **Distribution Date:** Set the date on which you want your rewards to be distributed.
- **Name your Snapshot:** Provide the name of your snapshot.

### Concentrated Liquidity Campaigns

For concentrated liquidity campaigns, Merkl allows you to allocate rewards based on three parameters:

1. **Fees**
2. **Token0** (e.g., for the pool USDA - ARB, USDA is token0)
3. **Token1** (e.g., for the pool USDA - ARB, ARB is token1)

Each parameter represents a percentage of the total rewards, so choose carefully based on what you want to incentivize. [This page of the docs](../mechanisms/campaigns/concentrated-liquidity-mechanisms.md) should give insight on good strategies to determine the parameters.

### ERC20, Lending and Borrowing, and Uniswap V2 like Campaigns

While you may see various campaign types associated to some lending or borrowing protocols, if a lending protocol or a constant product AMM doesn't have its card, you may still incentivize it directly on Merkl using the **ERC20 Campaign** type.

You may learn more about this super large campaign type [here](../mechanisms/campaigns/erc20-mechanisms.md).
In this case, make sure to check with us in advance though that the Merkl API will be able to properly compute APRs and TVLs associated to the protocol. It's not complex for us to support it, but we still have a small manipulation to do in this case.

For any of these campaign types, make sure that you understand the hooks well and be wary with the blacklist/whitelist as well as forwarding features here.

Lending and borrowing protocol incentives for the specifically integrated protocols can offer more tailored incentive mechanisms than the ones we have for vanilla ERC20 Campaigns.

#### Morpho Use Case

When creating a campaign for a Morpho pool, you'll be asked to input the following parameters:

- **Usage:** From the dropdown menu, select the usage type (_MetaMorpho_, _Supply_, _Borrow_, _Collateral_) you want to incentivize. Choosing _MetaMorpho_ will incentivize a MetaMorpho vault, while selecting _Supply_, _Borrow_, or _Collateral_ will incentivze a Morpho Blue market.
- **Market ID:** Specify the Market ID. For _MetaMorpho_, select the desired MetaMorpho vault to incentivize. For Morpho Blue, choose whether to incentivize the _supply_, _borrow_, or _collateral_ for a specific Morpho Blue market. You also have the option to manually enter the asset address for any type of Market ID.

#### Radiant Use Case

You'll be asked to set a `cap` parameter in USDC. This is used by Radiant; if you're not the Radiant protocol you should set it to 0.

#### Silo Use Case

Like on Morpho, there are some custom parameters to choose:

- **Usage:** Select the specific usage for your Silo campaign (Deposit, Protected Deposit, or Debt).
- **Version:** Choose the version of Silo (Silo Legacy, or Silo Llama).
- **Asset:** Select the asset to incentivize from the dropdown menu or enter the asset address manually.

You may also use the blacklist and whitelist features precisely:

- **Blacklist:** Automatically blacklist silos that should not receive or forward rewards by properly calibrating your markets in market configuration. This ensures that non-eligible silos are excluded from rewards distribution.
- **Whitelist:** Automatically whitelist a silo if the asset should be incentivized only for a specific market. Proper market calibration will fill this field accordingly.