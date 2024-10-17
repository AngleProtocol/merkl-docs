# 🏹 Airdrop Campaigns

Merkl supports airdrop campaigns, simplifying the process for protocols looking to airdrop tokens to their community by allowing them to get it done in a few minutes with just a few clicks while minimizing gas fees.

Merkl supports two different types of airdrops:

- one where incentive providers are responsible for allocating rewards to their users and choose which users get what amount of rewards. In this case, incentive providers come with a JSON file and Merkl is just used to handle the distribution through its usual claim system
- another one where Merkl airdrops the rewards based on the snapshoted balance of a given token at a chosen moment in time

## JSON airdrops

The only thing that is expected from reward providers is to come with a JSON file giving the breakdown of user addresses and the amount of rewards they need to receive.

The expected format for the JSOn is the following

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

### Important Note

On these campaigns, Merkl applies a fee of 0.5%. Therefore, if the total amount of tokens to be airdropped in your JSON file sums up to 100,000, then 100,000 will be distributed to the recipients, and a fee will be applied on top of it. To ensure that 100,000 tokens reach your users, you need to provide 100,502.512563 tokens (100,000 / (1 - 0.5%)).

Our frontend will automatically calculate the correct amount for you.

If you prefer to have exactly 100,000 tokens leave your wallet, then the sum of the amounts in your JSON file should be 99,500 tokens (100,000 \* (1 - 0.5%)).

## Token Balance Airdrops

In this type of airdrop, Merkl provides the possibility to airdrop tokens to token holders based on their snapshoted token balance at a chosen moment in time.

For this type of campaign, beyond the rewards amount, incentive providers need to input the following parameters:

- **Snapshot:** Set the time for the snapshot - it will be taken at the block prior to this date.
- **Distribution Date:** Set the date on which your rewards should be distributed.
- **Name your Snapshot:** Provide the name of their snapshot.
