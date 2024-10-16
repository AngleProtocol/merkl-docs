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
