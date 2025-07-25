description: This FAQ covers common questions from campaign creators before, during, and after launching an incentive program on Merkl.
---

# Merkl Campaign Creator FAQ

## Before creating a campaign

### Can users earn rewards even when they don’t hold the incentivized asset directly in their wallet?

Yes, thanks to forwarders (enabled by default), Merkl is able to distribute rewards to users holding an incentivized asset indirectly, such as through staking contracts or LP tokens.

### Should I include the distribution fees in my campaign budget?

Yes, only if you are creating an airdrop campaign. Your wallet will have to pay the distribution fees on top of the airdrop amount.

For other campaign types, the distribution fees will be taken from the rewards amount you previously defined.

### Can I test a campaign before launching it live?

Yes, we support test campaigns on mainnet using dummy tokens (e.g., Aglamerkl for point-based tests).

### How do I estimate the right budget and duration for my campaign?

You should consider your desired APR, the target TVL, and the campaign duration (we recommend a 2-week duration for your first campaigns).This will help you calculate the total reward amount you’ll need to deposit when launching your campaign. 

### What is the minimum reward distribution per hour?

Campaigns must distribute at least ~$1/hour in total rewards to avoid dusting. Also, positions with less than $10 in liquidity are excluded from receiving rewards.

### Do I need to whitelist the reward token beforehand?

Yes, if your token isn’t already available in the Merkl interface, please fill the form here with all the information (token name, address, price source, logo) so it can be whitelisted before launching your campaign.

### How long does it take for my campaign to appear on the frontend?

New campaigns may take up to 1 hour to appear on the Merkl App due to caching. If it’s still not visible after 1 hour, feel free to reach out to the team.

### Can I run multiple campaigns on the same asset/pool?

Yes, you can run several campaigns targeting the same asset/pool, with different reward tokens, or other parameters.

### Do ALMs take a cut of the rewards?

No, Merkl forwards 100% of the rewards to users. If ALMs (Automated Liquidity Manager) deduct fees or take a share, that’s up to their own contract logic.

## During the campaign

### Can I cancel a campaign and get refunded?

Yes, you can cancel a live campaign at any time. Any remaining (undistributed) tokens will be available for withdrawal through the Merkl UI or directly via contract interaction.

### What parameters can I edit after launch?

Editable parameters are start & end dates, opportunity name, customization options, blacklists and whitelists. You cannot change the reward token or reduce the total reward amount. However, you can increase the total reward amount by launching a new campaign.

### How often are rewards computed and distributed?

Reward computations (process by which the Merkl Engine calculates reward allocations) run on average every 2 hours. After this step, users can view the rewards they’ve earned, which are marked as "Claimable soon"

Rewards updates (process by which the Merkl Engine compresses computed campaign data into a Merkle root and pushes it onchain) run on average every 8 hours. After this step, users can claim their rewards. 

### Can I retrieve rewards earned by time?

We do not have a filter for points earned by time. However you can take a daily snapshot of our API to build this dataset.

### What do "Amount", "Pending", and "Claimed" mean in the UI?

- **Amount** refers to the total number of tokens already credited to the user onchain.
- **Pending** refers to rewards that are queued and will be pushed onchain in the next update..
- **Claimable** refers to rewards that have been computed and are ready to be claimed by users.
- **Claimed** shows the amount the user has already claimed.

### Why is APR or TVL not showing for my campaign / why do they have incorrect or unusual values ?

There can be several reasons for this. The main ones are:

- A missing token price — reach out to us on Telegram so we can add it!
- A cache issue — you need to wait up to one hour after creating a campaign for it to be visible in the app.
- The reward token price was entered incorrectly, leading to an inaccurate TVL calculation and, therefore, an incorrect APR.
- A technical issue occurred (subgraph, RPC, etc.). We’ll keep you informed once it’s resolved.
- An incorrect pool address or the wrong chain being selected when creating the campaign.

## During the campaign

### What happens to unclaimed rewards after the campaign ends?

Unclaimed tokens remain in the distributor's campaign vault. You can either withdraw them or allocate them to a new campaign.

### Can campaigns end prematurely?

Yes, only for fixed reward rates campaigns, when all the rewards have been distributed to users before the configured ending date. 
On the other hand, if too few users participate in the campaign, undistributed rewards will return to the campaign creator once campaigns end.

### Can I extend or relaunch a campaign?

Campaigns cannot be extended after they end, but you can easily create a new one using the same parameters. Any unused rewards from the previous campaign can be reused.

### Is there a way to retrieve the campaign data?

Yes. You can use our API to fetch detailed information. More information [here](https://docs.merkl.xyz/integrate-merkl/app). Here are the most commonly used endpoints:

- Getting the leaderboard for your campaign: https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/
- Getting opportunities and their included campaigns: https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/campaigns
- Checking rewards amount at user level: https://api.merkl.xyz/docs#tag/users/GET/v4/users/%7Baddress%7D/rewards
- Checking rewards amount at campaign level: https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total
- Checking rewards amount at token level: https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/
- Checking how many rewards are unclaimed: https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/
- Integrating APRs data in your front-end: https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/
    - For this route, match the incentivized asset’s address with the “explorerAddress” field.

Note: You can find each campaign or opportunity ID directly on the opportunities page [here](https://app.merkl.xyz/).