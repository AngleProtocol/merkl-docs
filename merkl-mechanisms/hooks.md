# 🪝 Customizability Hooks

Merkl allows incentive providers to customize campaign behavior using hooks, adding flexibility beyond standard campaign parameters.

Below are some of the most commonly used hooks in Merkl.

## 🔁 Forwarders

Merkl Engine intelligently enables users to earn rewards even when they don’t hold the incentivized asset directly in their wallet. For example, in a campaign rewarding token holders, many users may have their tokens staked in a contract. If Merkl has integrated that staking contract, those users will still receive rewards based on their staked amount.

This mechanism—tracking indirect ownership across contracts and protocols—is handled by Merkl forwarders. Forwarders allow Merkl to distribute rewards to users holding an incentivized asset indirectly, such as through staking contracts or LP tokens. By default, Merkl supports forwarding for most major DeFi protocols.

In most [campaign types](../mechanisms/hooks/mechanisms/), Merkl automatically detects and applies any integrated forwarders. However, for older, legacy campaigns (e.g., the original ERC20 campaign type), forwarders must be manually specified at the time of campaign creation.

**How It Works**:

* **Complexity Varies**: While for some simple forwarders, such as staking contracts for ERC20 tokens, the integration and forwarding process is relatively straightforward, some are equivalent to a complex integration of a new protocol type, and imply forwarding rewards across several stakeholders on multiple smart contract layers.
* **Auto-Detection**: Once a forwarder is integrated, it’s automatically applied—no manual configuration is needed. The Merkl frontend includes a scan tool to check if an address matches any known forwarder patterns.
* **Protocol Fidelity**: Merkl mirrors the logic of each protocol. For example, if a protocol charges a fee on accrued rewards, Merkl will automatically account for and replicate that fee in the reward forwarding process.

**Example 1: Staked Token Rewards**:

* A campaign incentivizes USDA holders.
* Users who staked USDA and received stUSD would normally be ineligible for rewards because they don't hold the USDA directly in their wallet.
* As forwarding is automatically enabled, Merkl recognizes stUSD holders as USDA holders and distributes rewards accordingly.

**Example 2: Morpho Rewards**:

* A campaign targets USDA holders.
* USDA is used across multiple Morpho markets, either as collateral or loan token.
* Merkl detects the Morpho singleton as a forwarder, automatically distributes the USDA rewards among relevant stakeholders across all markets, proportionate to their contribution to the singleton’s USDA holdings.

**Example 3: Pendle Rewards**:

* A campaign rewards USDA holders.
* USDA is deposited into a Pendle market.
* Merkl forwards rewards only to eligible Pendle stakeholders (YT and LP token holders), excluding PT holders, and applies the Pendle treasury fee per protocol rules.

**How to Enable Forwarding**:

Forwarding is enabled by default on most campaign types in Merkl. If your campaign requires a new forwarder integration or support for a new protocol, please reach out to our team.

{% hint style="info" %}
Coming soon: When creating a campaign, you’ll be able to specify that an address is an ERC20 token, enabling automatic forwarding to its token holders.
{% endhint %}

On the legacy campaign types where forwarders must be manually specified, when creating a campaign, you need to:

* Provide the staking contract address (recipient of the rewards).
* Provide the forwarded token address (token users receive when staking).

## ✅ Whitelisting

Whitelisting restricts rewards to a specific address or set of addresses (e.g., selected forwarders, ALMs, or individual users).

**Example: Uniswap V3 ALM Incentives**:

* A campaign incentivizes LPs in a Uniswap V3 pool with three Automated Liquidity Managers (ALMs).
* The incentive provider whitelists only two ALMs and one user address.
*   Result:

    ```
    - Only liquidity providers using the two approved ALMs or the whitelisted user will receive rewards
    - Rewards are distributed normally among whitelisted addresses based on liquidity share.
    ```

**Whitelisting & Blacklisting Priority**:

* Whitelisting overrides blacklisting: If an address is whitelisted, all other addresses are automatically blacklisted.
* If multiple campaigns run on the same pool, some may have whitelists while others do not.
* Users should check the campaign details to confirm eligibility requirements.

## ❌ Blacklisting

Blacklisting excludes specific addresses from receiving rewards.

* If a forwarder is blacklisted, all associated users are also ineligible.
* Example: Staking Contract Blacklist
  * A user holds 10 USDA but has staked 5 USDA in a blacklisted staking contract.
  * Only the 5 USDA in the user’s wallet qualifies for rewards.
  * Impact: Blacklisted addresses cannot earn rewards. The campaign rewards are distributed among eligible participants.

## 🚫 OFAC Compliance

Merkl provides an automatic OFAC sanctions blacklist hook, ensuring that any addresses flagged by the Office of Foreign Assets Control (OFAC) are excluded from all campaigns.

* The blacklist is continuously updated.
* Helps protocols comply with international sanctions.

## 🌉 Incentivized bridged liquidity

Merkl has partnered with Jumper to allow incentive providers to reward users for bridging liquidity from another chain.

**Why Use This?**:

* Instead of rewarding liquidity movement within the same chain, incentives can target cross-chain liquidity inflows.
* Helps chains attract new liquidity from external networks.

**How It Works**:

* Only users who bridged funds via a whitelisted bridge will be eligible.
* Ensures that only genuine cross-chain liquidity movement is rewarded.

## 🗯️ Boosting Rewards with Merkl

Merkl allows incentive providers to boost rewards for users holding a specific token or NFT.

* Similar to Curve’s vote-escrowed boost formula but with more flexibility.
* No 2.5x limit – You can customize boost multipliers as needed.

### Boost Formula Computation

$$
B = b \times \frac{R \times v}{V \times r} + 1
$$

Where:

* **B**: Boost multiplier
* **b**: Custom boost factor chosen by incentive provider
* **R**: Total rewards per epoch
* **r**: User’s reward per epoch
* **V**: Total supply of the boost token/NFT
* **v**: User’s holdings of the boost token/NFT

### NFT-Based Boosting

* Boosts can be based on NFT holdings.
* Contact us for help setting up NFT-based reward boosts.

## 🎯 Eligibility Filters

Merkl allows eligibility requirements based on:

* Minimum token holdings
* Holding duration

Example:

* A campaign requires 500 stUSD held for 30 days.
* A user holding 600 stUSD for 44 days qualifies.
* A user holding 400 stUSD for 60 days does not qualify.

This hook allows incentive providers to reward only addresses that meet both a minimum token holding requirement and a set duration. For example, if the token chosen for eligibility is stUSD, and the threshold is set to 500 stUSD with a duration of 30 days, a user holding 600 stUSD for 44 days would qualify for rewards.

Addresses that fail to meet either the token amount or duration threshold are excluded from the campaign’s rewards, ensuring incentives are focused on long-term participants rather than short-term holders.

## 🌍 Worldchain ID

Merkl enables filtering of rewarded users by leveraging Worldchain's identity system, [World ID](https://docs.world.org/world-id), which allows users to anonymously and securely verify that they are real and unique humans.

## 🔄 Dynamic Boosting – API Hook

Merkl provides several methods to manage dynamic boosting through an API. Here are the different methods available:

#### Multiply

Multiply the current amount by the provided input.

$$
\text{amount} = \text{amount} \times \text{boost}
$$

#### Multiply with Offset

Apply a 1 + boost computation.

$$
\text{amount} = \text{amount} \times (1 + \text{boost})
$$

#### Add

Add the boost number to the current amount.

$$
\text{amount} = \text{amount} + \left(\frac{\text{boost} \times \text{amount}}{\text{recipientsToAmount[recipient]}}\right)
$$

#### Replace

Replace the current amount with the boosted amount.

$$
\text{amount} = \left(\frac{\text{boost} \times \text{amount}}{\text{recipientsToAmount[recipient]}}\right)
$$

### Implementation

The hook has the following parameters:

* **url**: The endpoint to which the API call will be made.
* **boostingFunction**: The function used to calculate the boost. Options include `REPLACE`, `ADD`, `MULTIPLY`, and `MULTIPLY_WITH_OFFSET`.
* **sendScores**: A boolean indicating whether to send scores along with the addresses.
* **defaultBoost**: The default boost value to use if no specific boost is provided. Options include `ZERO_ADDRESS` and `ERROR`.
  * ZERO\_ADDRESS : If we don't find the address in your response, we will use the ZERO\_ADDRESS boost as a default value.
  * ERROR : If we don't find the address in your response, the campaign will not proceed.

Depending on whether `sendScores` is true or false, we will POST the following body along with the API call:`sendScores=True`

```jsx
let body: { address: string, score: string }[]
```

`sendScores=False`

```jsx
let body: { addresses: string[] }
```

We will expect the following response:

```jsx
const data : {
  address!: string;
  boost!: bigint;
}[]
```

Any other response will be dropped, and if the object cannot be parsed, the hook will fail.

## Default Values

Since we always exclude the zero address in our computation, we can use it to fill in the default values.

Campaign creators can also choose to throw an error instead of proceeding, which is a safer option.

## 🎟️ Raffles

### 🌶️ Spice up your rewards!

Merkl allows you to set up raffles that randomly select lucky winners for your campaigns. You can customize these raffles in various ways to match your campaign needs.

### Customisation

Merkl provides several options for you to tailor your raffles:

* **Mutliple raffles**: Choose how often you want raffles to run. Every day? Every week? The choice is yours
* **Number of winners**: Decide how many winners you want to select in each raffle. You can have one grand prize winner, or ay number of lucky winners, depending on your preference.
* **Selection method**: You can choose how winners are selected.
  * **Everyone is equal**: All participants have an equal chance of winning.
  * **Whales first**: Users with higher campaign scores have a better chance of winning (this can help reward top participants).
* **Multiple selection**: You can set up multiple raffles that run at the same time, each with its own rules on how rewards are distributed.

### Reproducibility

**Seed**\
The seed is the **block hash** of the very next block after the timestamp set by the campaign creator.

**Generating Winning Numbers**\
The random number generation is done using the **XORShift128Plus** pseudo-random number generator (PRNG). This PRNG is seeded with the previously generated seed.\
The **step** is the minimum score of a user divided by 1000. (In the case where the score is not used to pick users, the step is, of course, 1.)

```typescript
function getSelectedNumbers(
  seed: number,
  numberOfWinners: number,
  totalScore: number,
  step: number,
): number[] {
  const random = new XORShift128Plus(seed)
  const selectedNumbers = []
  for (let i = 0; i < numberOfWinners; i++) {
    selectedNumbers.push(random.randBelow(totalScore / step) * step)
  }
  return selectedNumbers
}
```

**Winners**\
The selected numbers correspond to different **"ranges"** in the list of participants (based on their amount or score). The system picks winners by matching these random numbers with ranges of amounts that participants have.\
Winners are picked by sorting all the participants by their address.

In the case of a **snapshot** or **airdrop**, obtaining the list of addresses is straightforward because the participants are known ahead of time. This list typically includes all the addresses that are eligible for the snapshot or airdrop, and they are usually collected from a specific event or condition.

For **more complex campaigns**, where winners are determined through specific criteria or weighted selections, one approach is to run a campaign with the **exact same parameters** but **without the raffle hook**.

The key idea is:

* You run this simplified version of the campaign **with a smaller amount** (i.e., a lower prize or allocation) and **without the raffle hook**.
* This will **generate the list of users** that Merkl identifies as eligible or **potential winners**.
* You can use this list to generate the list of users.

In essence, the result of this campaign (without the raffle hook) gives you the list of users who met the conditions set by the campaign. These users are then the ones Merkl considers for potential winning when the raffle hook is applied.

### How the Winner is Selected: Example with 5 Users, 5 Weights, and One Number Picked

#### Scenario Setup:

We have **5 users** in the raffle, each with an associated **weight** (which represents their chance of winning). The system will pick **one winner** based on a randomly selected number.

Let’s assume the users and their weights are as follows:

| User | Weight (Amount) |
| ---- | --------------- |
| A    | 10              |
| B    | 20              |
| C    | 30              |
| D    | 40              |
| E    | 50              |

We want to pick one winner randomly based on these weights.

### Step-by-Step Explanation:

#### 1. **Total Weight Calculation**:

First, we calculate the **total weight** by summing the weights of all the users. This total represents the "pool" from which the random number will be drawn.

Total Weight = 10 + 20 + 30 + 40 + 50 = 150

#### 2. **Random Number Generation**:

The system will then pick a random number. This random number will be between **0 and the total weight (150)**.

Let’s assume the randomly picked number is **107**.

#### 3. **Weighted Selection**:

The system uses this random number to determine which user will win. To do this, it calculates cumulative weights, which define "ranges" for each user.

* For **User A**: The range is from **0 to 10** (because A has a weight of 10).
* For **User B**: The range is from **10 to 30** (since A’s range is 0–10, and B has a weight of 20).
* For **User C**: The range is from **30 to 60**.
* For **User D**: The range is from **60 to 100**.
* For **User E**: The range is from **100 to 150**.

So, the cumulative ranges are as follows:

| User | Cumulative Weight Range | Range |
| ---- | ----------------------- | ----- |
| A    | 0–10                    | 10    |
| B    | 10–30                   | 20    |
| C    | 30–60                   | 30    |
| D    | 60–100                  | 40    |
| E    | 100–150                 | 50    |

#### 4. **Determine the Winner**:

Now, the system checks where the random number falls in the cumulative weight ranges:

* **Random number**: 107\
  The number **107** falls in **User E's** range (100–150), so **User E** is the winner.
