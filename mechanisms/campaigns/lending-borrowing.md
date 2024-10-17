# 🏦 Custom Lending and Borrowing Campaigns

While Merkl supports by default all lending and borrowing protocols that issue receipt tokens for lenders and debt tokens for borrowers, it also supports:

- lending protocols which are not structured this way
- more tailored campaigns around these protocols (e.g incentivizing the net lending positions of a user - discounting what has been borrowed and re-lent)

Some examples of custom lending/borrowing integrations include Silo, Radiant, Morpho, Euler.

From a user perspective, the experience is exactly the same with these specific lending protocols as it is with more commonly structured lending/borrowing platforms: the Merkl engine abstracts away the difference so the UX and the features supported are typically the same.
Typically, in these protocols as well, the default mode is one where users are rewarded based on their time weighted lent or borrowed amount relative to what's been lent or borrowed overall.

The important thing to note is that everything that relates to blacklisting and forwarding mentioned [for ERC20 campaigns](./erc20-mechanisms.md) also applies in these situations.
Typically, if people never directly lend in a protocol but can only lend through curated vault, then Merkl forwarding technology can automatically take this into account and only reward users which lent through a vault.

{% hint style="info" %}
If you want your lending and borrowing protocol to be fully integrated and supported by Merkl, or to create tailored protocol-campaigns (as we did for Silo and Radiant), please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process.
{% endhint %}

Here are below some specificities you may face with some lending protocol integrations on Merkl.

## Morpho

When creating a campaign for a Morpho pool, you'll be asked to input the following parameters:

- **Usage:** From the dropdown menu, select the usage type (_MetaMorpho_, _Supply_, _Borrow_, _Collateral_) you want to incentivize. Choosing _MetaMorpho_ will incentivize a MetaMorpho vault, while selecting _Supply_, _Borrow_, or _Collateral_ will incentivze a Morpho Blue market.
- **Market ID:** Specify the Market ID. For _MetaMorpho_, select the desired MetaMorpho vault to incentivize. For Morpho Blue, choose whether to incentivize the _supply_, _borrow_, or _collateral_ for a specific Morpho Blue market. You also have the option to manually enter the asset address for any type of Market ID.

## Radiant

You'll be asked to set a `cap` parameter in USDC. This is used by Radiant: if you're not the Radiant protocol you should set it to 0.

## Silo

Like on Morpho, there are some custom parameters to choose:

- **Usage:** Select the specific usage for your Silo campaign (Deposit, Protected Deposit, or Debt).
- **Version:** Choose the version of Silo (Silo Legacy, or Silo Llama).
- **Asset:** Select the asset to incentivize from the dropdown menu or enter the asset address manually.

The blacklist/whitelist features also come handy here as you may use it to automatically blacklist silos that should not receive rewards.
