---
description: Guide to create a Merkl Campaign from a DAO
---

# Create Your Campaign from a DAO

You can create campaigns directly using onchain smart contracts with Merkl. This guide covers different methods for doing so.

## Prerequisites

All methods require generating the associated data for your campaigns.

To retrieve the necessary campaign data and parameters, check our guides for using [Merkl Studio](./create-a-campaign.md) or [the Merkl APIs programmatically](./create-multiple-campaigns.md). In either case, the campaign data can be found in the campaign JSON payload you would receive when creating campaigns from a multisig.

## From an Onchain Gauge System

Many protocols rely on gauge systems where users vote onchain to determine the allocation of governance token rewards.

Merkl offers a set of plugins to facilitate the integration of Merkl campaign creation within such systems. These plugins combine Merkl's flexibility and reporting features with the advantages of a fully automated onchain setup.

### How It Works

These plugins are smart contracts that expose the same interfaces as standard staking contracts for creating new campaigns (like `notifyReward`). When called, these contracts receive the reward tokens (via `transferFrom`) and create Merkl campaigns directly based on pre-set parameters.

For these plugins to work, they must first be initialized with campaign data—such as which pool to incentivize, duration, blacklist/whitelist settings, and other parameters.

Once set up, campaigns can be created automatically (e.g., weekly) by a bot with no trust assumptions—the process can even be made permissionless.

Each week, based on DAO votes or the voting contract configuration, the plugin uses predefined parameters and updates the reward amount (based on voting results) and start timestamps when called.

An example plugin can be found [here](https://github.com/AngleProtocol/merkl-contracts/blob/main/contracts/partners/middleman/MerklGaugeMiddlemanTemplate.sol).

Other teams have also developed different implementations. Check out these examples:

- [Quickswap](https://polygonscan.com/address/0x3a381497813208508689d78c90EC9fb115D5640d#code)
- [CrossCurve implementation](https://arbiscan.io/address/0xb665d0B69e91F596B9Dee3016e49136335993Fb8#readProxyContract)

### Step-by-Step Guide

**1. Update the Plugin Contract Interface**

The plugin example assumes that the voting contract calls the gauge with the `notifyReward` function.

Your voting contract may call associated gauges with different parameters. Ensure that the interface exposed by the plugin contract matches the one used by your other gauges.

Your onchain governance may only support one reward token instead of multiple. You may need to update the plugin contract accordingly, as the Merkl template is designed to accommodate multiple reward tokens.

Note: Depending on your voting contract setup, you can deploy one contract for all of your gauges.

**2. Define Your Campaign Parameters**

Once your contract is deployed, pre-format your campaigns using the `setGaugeParameters` function ([reference](https://github.com/AngleProtocol/merkl-contracts/blob/32150c5577127a1feaf0315e1d7fcca0830eeb3c/contracts/partners/middleman/MerklGaugeMiddlemanTemplate.sol#L65)).

**3. Automate Weekly (or Bi-weekly) Execution**

Once your plugin is set up and your campaigns are correctly pre-formatted (a one-time setup), you only need to configure a mechanism within your voting contract to trigger execution automatically at your desired frequency.

If you already have automation to push rewards to your other gauges, no additional setup is needed!

## From a One-Off Governance Proposal

Some protocols may need to execute one-off governance votes through onchain systems to deploy campaigns (typically for airdrops).

To successfully deploy your campaign through onchain governance, you'll need to create a campaign payload, then use the [Foundry Forge CLI](https://book.getfoundry.sh/forge/) to generate the calldata for a DAO proposal.

Follow these steps to build the payload and finalize your campaign creation.

{% hint style="info" %}
The example below is for the _Arbitrum network_. Be sure to select [the correct address](../integrate-merkl/smart-contract-addresses.md) for the Creator contract on your network.
{% endhint %}

**1. Create the Approval Transaction**

Approve the Creator contract to spend tokens. Remember to include the Merkl fee in your calculation.

```bash
cast calldata "approve(address,uint256)" 0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd 35175873467336683417086
```

Then use the output to craft the proposal action:

```json
{
  "target": "0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c",
  "calldata": "0x095ea7b30000000000000000000000008bb4c975ff3c250e0ceea271728547f3802b36fd000000000000000000000000000000000000000000000772e34ed4c71327edfe"
}
```

**2. Create the Accept Merkl Terms Transaction**

Accept the Merkl Terms of Service:

```bash
cast calldata "acceptConditions()"
```

Example result:

```json
{
  "target": "0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd",
  "calldata": "0x63b87bf0"
}
```

**3. Copy and Edit the Payload**

Follow the guides mentioned above to generate and copy the campaign payload.

Here is the function selector for reference:

```js
createCampaign(
  tuple(
    bytes32, // campaignId
    address, // creator
    address, // rewardToken
    uint256, // amount
    uint32, // campaignType
    uint32, // startTimestamp
    uint32, // duration
    bytes, // campaignData
  ),
)
```

```js
;[
  '0x0000000000000000000000000000000000000000000000000000000000000000',
  '0x0000000000000000000000000000000000000000',
  '0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c',
  '35175873467336683417086',
  4,
  1734307200,
  3600,
  '0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000005568747470733a2f2f616e676c652d626c6f672e696e667572612d697066732e696f2f697066732f516d5558667658666274526576376d377750796a464d4c736a777568457a676e796a334653376172714b69446f33000000000000000000000000000000000000000000000000000000000000000000000000000000000000194f70656e20446f6c6c617220626f6c74732072657761726473000000000000000000000000000000000000000000000000000000000000000000000000000000',
]
```

Then update the format to be compatible with the Foundry CLI:

- Replace square brackets `[` with parentheses `(`
- Remove quotation marks `"`
- Remove newlines
- Add single quotes `'` around the entire payload

For example:

```js
'(0x0000000000000000000000000000000000000000000000000000000000000000,0x0000000000000000000000000000000000000000,0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c,35175873467336683417086,4,1734307200,3600,0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000005568747470733a2f2f616e676c652d626c6f672e696e667572612d697066732e696f2f697066732f516d5558667658666274526576376d377750796a464d4c736a777568457a676e796a334653376172714b69446f33000000000000000000000000000000000000000000000000000000000000000000000000000000000000194f70656e20446f6c6c617220626f6c74732072657761726473000000000000000000000000000000000000000000000000000000000000000000000000000000)'
```

**4. Create the `createCampaign` Transaction**

Run the cast command using the properly formatted payload:

```bash
cast calldata "createCampaign(tuple(bytes32,address,address,uint256,uint32,uint32,uint32,bytes))" '<YOUR_PAYLOAD>'

// Example
cast calldata "createCampaign(tuple(bytes32,address,address,uint256,uint32,uint32,uint32,bytes))" '(0x0000000000000000000000000000000000000000000000000000000000000000,0x0000000000000000000000000000000000000000,0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c,35175873467336683417086,4,1734307200,3600,0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000005568747470733a2f2f616e676c652d626c6f672e696e667572612d697066732e696f2f697066732f516d5558667658666274526576376d377750796a464d4c736a777568457a676e796a334653376172714b69446f33000000000000000000000000000000000000000000000000000000000000000000000000000000000000194f70656e20446f6c6c617220626f6c74732072657761726473000000000000000000000000000000000000000000000000000000000000000000000000000000)'
```

The third and final transaction will look like this:

```json
{
  "target": "0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd",
  "calldata": "0xa63f05ad000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000d636bd52bfc1b3a699165ef5aa340bea8939c000000000000000000000000000000000000000000000772e34ed4c71327edfe000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000675f6d800000000000000000000000000000000000000000000000000000000000000e1000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000005568747470733a2f2f616e676c652d626c6f672e696e667572612d697066732e696f2f697066732f516d5558667658666274526576376d377750796a464d4c736a777568457a676e796a334653376172714b69446f33000000000000000000000000000000000000000000000000000000000000000000000000000000000000194f70656e20446f6c6c617220626f6c74732072657761726473000000000000000000000000000000000000000000000000000000000000000000000000000000"
}
```
