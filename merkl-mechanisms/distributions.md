---
description: Merkl different distribution types
---

# Distribution Types

Merkl supports various distribution methods, allowing campaign creators to choose how rewards are allocated based on participant activity and liquidity contributions.

## 📈 Variable Reward Rate Campaigns

In variable reward rate campaigns, rewards are distributed proportionally based on time-weighted liquidity within the eligibility pool.

This is the most classical form of campaign.

### Key Characteristics

* Constant reward rate for campaign creators: A fixed amount of tokens is distributed per second, regardless of liquidity changes.
* Variable APR for participants: As more users join, rewards per user decrease (like slicing a fixed-size pizza into more pieces).
* Prefunded & fixed duration: The full campaign budget is deposited upfront, and the campaign runs for a set time.

### Example: Liquidity Pool Incentives

Imagine an incentive program targeting liquidity in a pool:

* Day Start: Two LPs each contribute $100 (50%/50%).
* Midday: A third LP adds $200 (total pool: $400).

As a result:

* The first two LPs each get 50 tokens for their contribution in the first half of the day, then 25 tokens each for the second half.
* The third LP, who contributed $200 in the second half, receives 50% of the rewards for that period, which is 50 tokens.

Result: Early participants earn more rewards before new LPs enter, while later participants dilute the total yield for others.

### Anti DoS Measures

To prevent excessive computational load and spam, Merkl applies an anti-DoS filter to Variable APR campaigns.

* Users earning less than 0.000001% of total campaign rewards per engine run are excluded.
* This prevents micro-distributions that could disrupt system performance.

Example: Uniswap V2 Liquidity Pool Campaign

* Campaign Duration: 14 days
* Total Value Locked (TVL): $1,000,000 (on Arbitrum)
* Merkl Engine Run Frequency: Every 4 hours (6 times per day)

For this campaign:

* Each reward distribution releases 1/84th of the total rewards (1 / (14 days × 6 runs per day)).
* If a user provides $10 of liquidity, they represent 1/100,000th (0.001%) of the total TVL ($10 / $1,000,000).
* Per engine run, their potential reward would be 1/8,400,000th (0.000012%) of the total rewards (1/100,000 × 1/84).
* Since this is above the 1/100,000,000th (0.000001%) threshold, the reward would be distributed.

However, if the TVL increased to $20,000,000, the same user’s share would drop to 1/2,000,000th (0.00005%), and their per-run reward would be 1/16,800,000th (0.0000006%), below the threshold. As a result, they would not receive rewards.

## 🔒 Fixed Reward Rate Campaigns

In fixed reward rate campaigns, users earn a predefined amount of rewards per unit of liquidity at a set rate.

### Key Characteristics

* Constant reward rate per users: Users receive a fixed APR regardless of total liquidity.
* Variable cost for campaign creators: As more users join, the overall cost increases, but individual earnings remain stable.
* Campaign may finish earlier: Campaigns end when funds are depleted or when the scheduled duration expires. If a campaign runs out of funds before its end date, rewards are split proportionally (like a variable reward rate campaign) in the final run. If the campaign ends early, unused rewards are returned to the campaign creator.

**There are two types of fixed reward rate campaigns, depending on whether the distributed amount is pegged to a fixed token amount or to a fixed dollar value.**

* **Fixed token reward**
  * Users earn a fixed amount of tokens, regardless of token price variations
  * Users’ APR is correlated to token reward’s price change
  * Total rewards amount may or may not be fully spent, depending of user’s participation
* **Fixed dollar reward**
  * Users earn a fixed amount of dollars (distributed in tokens)
  * APR in dollars is fixed for users (but the token amount may vary)
  * The amount of token distributed adjusts with token price variations (but the USD-denominated value is fixed)

### Example: Fixed Reward Rate Campaign

A campaign offers $1 in rewards per $100 in liquidity per day - which makes a 365% fixed APR.

* If TVL = $100, the cost is $1 per day.
* If TVL = $100 million, the cost for the campaign creator scales to $1M per day.

You can also incentivize by the number of tokens held. Let's say that Bob wants to incentivize holding _LONG_ and he doesn't know how to price it.

A campaign can offer $1 in rewards per 100 _LONG_ held per day.

### 🎯 The Special Case of Points

This distribution method is widely used in point systems. Traditionally in points systems, users earn points for completing a specific action (like providing $1 of liquidity in a pool for a day), regardless of how many other people did.

The main difference here in fixed APR campaigns for points is that when distributing points, the budget is effectively unlimited; and so campaign creators can allocate as many points as they want, allowing the campaign to last indefinitely.

#### Example: Points

Bob wants to incentivize holding his token _LONG_ with a point token named _BOB_.

If _LONG_ has a dollar value, Bob can create a campaign that offers 1 _BOB_ in rewards per $1 held of _LONG_ in liquidity per day. Alternatively, Bob can create a campaign that offers 1 _BOB_ in rewards per X _LONG_ held in liquidity per day, when he doesn't want to take into account the dollar value of _LONG_.

## Coming Soon: Max APR campaigns

These campaigns are variable APR campaigns which become fixed APR campaigns when they start distributing too much.

## ⚠️ Important Note

While Merkl distribution types vary in how rewards accrue and distribute over time—whether linearly or non-linearly—one key factor remains the same:

* New rewards only become claimable after a Merkle root update.
* These updates occur at fixed intervals, which vary by chain (typically every 4 to 12 hours, with an average of 8 hours).

**Example in a Variable APR campaign**:

* You create a 14-day campaign with a $42,000 budget.
* Merkle root updates occur every 8 hours (3 times per day).
* Total updates during the campaign: 42 runs (14 days × 3 runs/day).
* Rewards per update:
  * $42,000 ÷ 42 runs = $1,000 per run.
  * Users can claim $1,000 from the allocated rewards at each update.

Although rewards are continuously calculated, they only become available for claiming when the Merkle root is updated onchain.
