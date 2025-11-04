# Reward Forwarding

Merkl Engine intelligently enables users to **earn rewards even when they don’t hold the incentivized asset directly in their wallet**. For example, in a campaign rewarding token holders, many users may have their tokens staked in a contract. If Merkl has integrated that staking contract, those users will still receive rewards based on their staked amount.

This mechanism—tracking indirect ownership across contracts and protocols—is handled by Merkl forwarders. Forwarders allow Merkl to distribute rewards to users holding an incentivized asset indirectly, such as through staking contracts or LP tokens. By default, Merkl supports forwarding for most major DeFi protocols.

In most [campaign types](../mechanisms/hooks/mechanisms/), Merkl automatically detects and applies any integrated forwarding logic. 

<figure><img src="../.gitbook/assets/Capture d’écran 2025-10-10 à 15.20.07 1.png" alt=""><figcaption><p>The forwarder scan in Merkl Studio</p></figcaption></figure>

{% hint style="info" %}
Check whether a contract is recognized as a forwarder by Merkl at [https://forwarders.merkl.xyz/](https://forwarders.merkl.xyz/)
{% endhint %}

**How It Works**:

* **Complexity Varies**: While for some simple forwarders, such as staking contracts for ERC20 tokens, the integration and forwarding process is relatively straightforward, some are equivalent to a complex integration of a new protocol type, and imply forwarding rewards across several stakeholders on multiple smart contract layers.
* **Auto-Detection**: Once a forwarder is integrated, it’s automatically applied—no manual configuration is needed. The Merkl frontend includes a scan tool to check if an address matches any known forwarder patterns.
* **Protocol Fidelity**: Merkl mirrors the logic of each protocol. For example, if a protocol charges a fee on accrued rewards, Merkl will automatically account for and replicate that fee in the reward forwarding process.

<figure><img src="../.gitbook/assets/Docs-merkl-forwarders.png" alt=""><figcaption></figcaption></figure>

**Example 1: Staked Token Rewards**:

* A campaign incentivizes USDA holders.
* Users who staked USDA and received stUSD would normally be ineligible for rewards because they don't hold the USDA directly in their wallet.
* As forwarding is automatically enabled, Merkl recognizes stUSD holders as USDA holders and distributes rewards accordingly.

**Example 2: Morpho Rewards**:

* A campaign targets USDA holders.
* USDA is used across multiple Morpho markets, either as collateral or loan token.
* Merkl detects the Morpho singleton as a forwarder, automatically distributes the USDA rewards among relevant stakeholders across all markets, proportionate to their contribution to the singleton’s USDA holdings.

**Example 3: Pendle Rewards**:

* A campaign rewards sUSDe holders.
* Merkl rewards sUSDe "spot" holder and forwards rewards only to eligible Pendle stakeholders (YT and LP token holders), excluding PT holders, while applying the standard Pendle treasury fee.

**How to Enable Forwarding**:

Forwarding is enabled by default for most campaign types on Merkl. If your campaign requires integration with a new forwarder or support for a different protocol, please contact our team.

To ensure efficient distribution, Merkl enforces a minimum distribution threshold for each reward token. Campaigns can only be created if the token amount meets or exceeds this threshold. The same rule applies to forwarding: rewards are only forwarded if they pass the threshold.

Forwarding will not be enabled for an address if the total rewards over a given period fall below the minimum per-hour threshold for that token. For example, if an ERC20 vault receives just $0.01 of rewards in a day and the token’s threshold is $0.10 per hour, those rewards will not be forwarded. Instead, they’ll remain accrued at the vault address.

{% hint style="info" %}
Coming soon: When creating a campaign, you’ll be able to specify that an address is an ERC20 token—enabling automatic forwarding to its token holders.
{% endhint %}