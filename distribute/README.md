---
description: Guide for Merkl incentivizors
---

# 💰 Create an incentivization campaign with Merkl

DAOs or individuals looking to distribute incentives through Merkl need to create Merkl campaigns. There are three ways of creating a Merkl incentivization campaign:

- Through the [create page](https://app.merkl.xyz/create) of the Merkl App. This approach is recommended for first campaign creation and incentivizors who want to create few campaigns
  - Anyone who wants to create campaigns should visit this page at least once, it will show you all the campaign types and all the custom rules you can create for your campaigns.
- [From a Gnosis Safe multisig](#deploy-your-campaign-from-a-multisig-or-a-gnosis-safe), this is the approach we recommend. The whole setup is documented in the next section
- Directly from the [`DistributionCreator` contract](./supported-chains-amms.md) on the chain of your choice

Regardless of the method you are using, before depositing any incentives, make sure that:

- You have read and agreed to the T&Cs. The `DistributionCreator` contract will require you to sign this T&Cs and post your signature onchain. This is a one time requirement that you'll only need to do once when creating your first distribution.
- The token you want to distribute has been whitelisted and the amount per hour of tokens you want to distribute is greater than the minimum amount allowed in the contract (more on this below)
- The address you're looking to incentivize is supported on the chain you want to use

{% content-ref url="incentivizor-tc.md" %}
[incentivizor-tc.md](incentivizor-tc.md)
{% endcontent-ref %}

{% hint style="info" %}
Once created, a campaign on Merkl may take up to 1 hour to be picked up by the Merkl API and front interface. You may track how rewards were distributed to LPs of the pool by checking this [website](https://rewards.merkl.xyz/) which contains the history of rewards given across all distributions across all pools of all supported AMMs.
{% endhint %}

{% hint style="info" %}
If you want Merkl to integrate a new chain, a new AMM, or a new liquidity manager, check [this page](https://app.merkl.xyz/partner)
{% endhint %}

## Configure your campaign

The campaign configuration is specific to the type of campaign you want to create, however all campaign types share the following parameters:

- Campaign creator: overrides the address which created the campaign. The creator address is used to recover the funds if the campaign was misconfigured
- URL: url of the page to which the users will be redirected when clicking on the opportunity link on the Merkl frontend.
- Whitelist: Empty by default. If addresses are provided, only these addresses will accrue rewards. The behavior can change if the address is provided is handled by a forwarder (e.g. if you provide the address of a staking contract for ERC20 campaigns, all users who staked their tokens on the contract will receive rewards, but if you provided an EOA then only that EOA would receive rewards).
- Blacklist: Empty by default. List of addresses that will be not be taken into account during reward distribution. Similarly to whitelist, the behavior can be altered by forwarders.

Forwarders are explained in more details [here](../merkl-mechanism.md#merkl-forwarders) and in the campaign specific documentation.

The other parameters are specific to each campaign type:

- [Concentrated liquidity campaign configuration](./campaign-specific/clamm.md#campaign-configuration)
- [ERC20 campaign configuration](./campaign-specific/erc20.md#campaign-configuration)
- [ERC20 Snapshot campaign configuration](./campaign-specific/snapshot.md#campaign-configuration)
- [Airdrop campaign configuration](./campaign-specific/airdrop.md#campaign-configuration)

## Deploy your campaign from a multisig or a Gnosis Safe

The recommended method of interaction to distribute rewards with Merkl with a multisig is to use Gnosis Safe Transaction Builder.

### Signing the Terms & Conditions

Merkl requires people depositing incentives on the system to accept the T&Cs, this is done by calling the `acceptConditions` function of the `DistributionCreator`. This transaction is included in all subsequent transaction batches.

{% file src="../.gitbook/assets/acceptConditions.json" %}
Merkl Multisig Transaction Batch to accept T&Cs
{% endfile %}

### Gnosis Safe setup

If you don't have one already, you will first need to [create a Gnosis safe](https://docs.gnosischain.com/tools/wallets/safe).

Open your safe, go to `New transaction` and click on `Transaction Builder`.

Download our Multisig payload template:
{% file src="../.gitbook/assets/createCampaignSafeTemplate.json" %}
Merkl Multisig Transaction Batch to create campaigns
{% endfile %}

Upload the json file you've just downloaded in your transaction builder. The app might tell you that the transaction batch is from another chain, you can disregard this message (the Merkl smart contract addresses are the same on all chains).

You will see 3 transactions:

- `approve`: approval for the MerklDistributor contract to spend the reward tokens
- `acceptConditions`: transaction to accept Merkl T&Cs, it is mandatory to have this transaction the first time you create a campaign, you can remove it later if you wish.
- `createCampaign`: transaction to create the campaign

{% hint style="info" %}
If you want to create multiple campaigns in a single transaction batch, you can edit the json file and copy/paste the `approve` and `createCampaign` transactions in the transactions list. Once this is done you can upload the file again.
{% endhint %}

Update the `approve` transaction:

- change the `To Address` to the address of the token you want to distribute in the campaign
- change the `amount` to the quantity of token you want to distribute (don't forget the decimals)

Update the `createCampaign` transaction with the new campaign data. Refer to the [Building the payload](#building-the-payload) section to generate this data.

{% hint style="info" %}
Don't forget to save the transaction batch in your library so you can create new campaigns faster next time!
{% endhint %}

### Building the payload

Now that the signing requirement is clear, what payload should you set in the Gnosis Safe transaction builder?

#### From the Merkl Frontend (recommended)

Go the the [Merkl campaign creation page](https://app.merkl.xyz/create) and fill in all the data to configure your campaign.

Once this is done, click on the `Generate Gnosis Safe Payload` at the bottom of the page and copy the payload to your clipboard.

Copy past the payload in the `newCampaign` field of the `createCampaign` transaction.

#### Using the AngleProtocol sdk

The `campaign` tuple given for the `createCampaign` function has the following form:

```json
[
  // campaignId: this value must be left as is and cannot be customized
  "0x0000000000000000000000000000000000000000000000000000000000000000",
  // creator: Address that will "own" the campaign. It is the address that will be used to recover the funds if the campaign was mis-configured. If the null address is provided, the contract will set the creator to the address which created the campaign.
  "0x0000000000000000000000000000000000000000",
  // rewardToken: Address of the reward token (here the OP token) which will be distributed to the users
  "0x4200000000000000000000000000000000000042",
  // amount: Total amount of tokens to distribute
  "2000000000000000000000",
  // campaignType: depends on what you are going to incentivize -> CLAMM: 2, ERC20: 1
  2,
  // startTimestamp: timestamp of when the campaign should start
  1708519672,
  // duration: duration of the campaign in seconds (one week in this example)
  604800,
  // campaignData: encoded bytes with the custom rules of the campaign. Each campaign type uses different campaignData
  "0x0000000000000000000000001a7e4e63778b4f12a199c062f3efdd288afcbce800000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000000e000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"
]
```

After using the payload provided in example and customizing both the approval and the parameters of the `createCampaigns` transaction to fit your needs, you should be ready to execute the transaction to distribute rewards to Merkl!

## Integrate Merkl campaign information and user rewards in your frontend

The Merkl API provides all the information you need to show your users all Merkl related information. It contains data about the campaigns, the rewards received by the users and the data needed to craft claim transactions.

The Merkl API is documented [here](./integrate/api-doc.md)
