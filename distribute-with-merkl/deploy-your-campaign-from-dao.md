---
description: Guide to deploy a Merkl Campaign from a DAO
---

# 🗳️ Deploy your campaign from a DAO

## For an onchain gauge system

Many protocols rely on gauge systems where users vote onchain to determine the allocation of governance token rewards.

Merkl offers a set of plugins to facilitate the integration of Merkl campaign creation within such systems. These are designed to offer the best of Merkl's flexibility and reporting features combined with the advantages of a fully automated onchain setup.

### How it works

These plugins are smart contracts that expose the same interfaces as usual staking contracts when it comes to creating new campaigns (like `notifyReward`). When called, these contracts take the reward tokens (`transferFrom` the caller address) and create Merkl campaigns directly based on pre-set parameters for the campaign data.

For these to work, these contracts must first be initialized with data about the campaigns they will create—such as which pool to incentivize, duration, blacklist/whitelist settings, and other parameters.

Once set up, campaigns can then be created automatically every week by a bot, with no trust assumptions towards the bot (it can even be made permissionless).

Every week, based on what was voted by the DAO or on the voting contract, when called, the voting contract will take the predefined parameters and update the reward amount (based on the vote) and the start timestamps.

An example plug-in can be found [here](https://github.com/AngleProtocol/merkl-contracts/blob/main/contracts/partners/middleman/MerklGaugeMiddlemanTemplate.sol).

Other teams have also worked on different implementations. You may check some other examples here:

- [Quickswap](https://polygonscan.com/address/0x3a381497813208508689d78c90EC9fb115D5640d#code)
- [CrossCurve implementation](https://arbiscan.io/address/0xb665d0B69e91F596B9Dee3016e49136335993Fb8#readProxyContract)

### Step by step guide

1. **Update the plug-in contract interface**

The plugin example assumes that the voting contract calls the gauge with the `notifyReward` function.

Your voting contract may call associated gauges with different parameters. You must ensure that the interface exposed by the plugin contract matches the one used by your other gauges.

Your onchain governance vote may only work with one reward token instead of multiple. You may need to update the plugin contract accordingly, as the Merkl template is designed to accommodate multiple reward tokens.

Technically (but this depends on the setup of your voting contract), you can deploy one contract for all of your gauges.

2. **Define your campaign parameters**

Once your contract is deployed, the way you can pre-format your campaign is through the `setGaugeParameters` function [here](https://github.com/AngleProtocol/merkl-contracts/blob/32150c5577127a1feaf0315e1d7fcca0830eeb3c/contracts/partners/middleman/MerklGaugeMiddlemanTemplate.sol#L65).

To retrieve the necessary campaign data and parameters for this function based on your use case, follow the guide [here](./deploy-your-campaign-from-a-multisig-or-gnosis-safe.md).

3. **Automate weekly (or biweekly) execution**

Once your plugin is set up and your campaigns are correctly pre-formatted (a one-time setup), you only need to configure a mechanism within your voting contract to trigger execution automatically each week.

If you already have an automation setup to push rewards to your other gauges, then there's nothing specific that you need to do!

## For a governance proposal

Some protocols may also need to execute one-off governance votes through onchain systems to execute campaigns (can be airdrops typically).

To successfully deploy your campaign through an onchain governance, you will need to create a campaign payload, then use the [Foundry Forge CLI](https://book.getfoundry.sh/forge/) to generate the calldata for a DAO proposal.

Follow these steps to build the payload and finalize your campaign creation:

The example below is for _Arbitrum network_. Be sure to select the correct address for the Creator contract on your network: https://app.merkl.xyz/status

1. **Create the approval transaction**

Approve the Creator contract to spend tokens. Don't forget the Merkl fee in your calculation.

```bash
cast calldata "approve(address,uint256) 0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd 35175873467336683417086
```

Then use the output to craft the proposal action, for example:

```json
{
  "target": "0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c",
  "calldata": "0x095ea7b30000000000000000000000008bb4c975ff3c250e0ceea271728547f3802b36fd000000000000000000000000000000000000000000000772e34ed4c71327edfe"
}
```

2. **Create the ToS transaction**
   Sign the Merkl terms of service

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

3. **Copy and edit the payload**
Follow the instructions in [🧑‍🔬 Deploy your campaign from a multisig or Gnosis Safe](distribute-with-merkl/deploy-your-campaign-from-a-multisig-or-gnosis-safe.md) to generate and copy the campaign payload. This step is required to properly generate the `campaignData`, which includes the campaign IPFS hash.
<figure><img src="../.gitbook/assets/Build-the-payload.png" alt=""><figcaption></figcaption></figure>
The campaign becomes available at `startTime + duration`. NOTE: `duration` is NOT the length of time in which users can claim. 
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

- Replace the square brackets `[` with parenthesis `(`
- Remove quotations `"`
- Remove newlines
- Add single quotes `'`
  For example:

```js
'(0x0000000000000000000000000000000000000000000000000000000000000000,0x0000000000000000000000000000000000000000,0x000D636bD52BFc1B3a699165Ef5aa340BEA8939c,35175873467336683417086,4,1734307200,3600,0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000005568747470733a2f2f616e676c652d626c6f672e696e667572612d697066732e696f2f697066732f516d5558667658666274526576376d377750796a464d4c736a777568457a676e796a334653376172714b69446f33000000000000000000000000000000000000000000000000000000000000000000000000000000000000194f70656e20446f6c6c617220626f6c74732072657761726473000000000000000000000000000000000000000000000000000000000000000000000000000000)'
```

4. **Create the `createCampaign` transaction**
   Run the cast command using the the properly formatted payload

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
