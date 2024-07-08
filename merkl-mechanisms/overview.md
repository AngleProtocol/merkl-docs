# Overview

Merkl is a platform designed to streamline and optimize reward distribution in the DeFi space. Merkl can handle multiple incentive providers for the same campaign type, allowing for complex reward structures and broad participation incentives. Here's a step-by-step overview of how Merkl works:

1. **Campaign Creation**
   * An incentive provider creates a campaign using the Merkl Distribution Creator contract. This includes specifying the rules and conditions under which rewards will be distributed. The campaign details are then pushed on-chain, ready to start tracking eligible user activity.
2. **User Participation**
   * Users interact with the protocol in various ways (e.g., providing liquidity, lending,  borrowing, etc.) to become eligible for rewards as defined by the campaign rules.
3. **Reward Computation**
   * The Merkl Engine, an off-chain component hosted by Angle Labs, regularly collects on-chain and off-chain data to measure user behavior. At fixed intervals, the Merkl Engine processes the collected data to update reward distributions. This ensures that users can claim new rewards multiple times a day without needing to stake tokens or perform additional on-chain actions. Based on the data, the engine calculates the reward distribution, aggregates it into a merkle tree, and compresses it into a merkle root. The computed merkle root is then pushed on-chain. Following the merkle root update, a dispute period allows anyone to verify the reward distribution and contest it if necessary.
4. **Reward Distribution**
   * Once the dispute period is over and the merkle root is validated, users can claim their rewards. The rewards from potentially multiple campaigns are aggregated, enabling users to claim all their rewards in a single transaction. Users can track their active positions and rewards through the [Merkl user dashboard](https://app.merkl.xyz/user), providing a transparent view of their earnings and investments.

For a more detailed and technical explanation of the Merkl protocol, visit the [Architecture and Technical Overview page](architecture-and-technical-overview/) for an in-depth examination of Merkl's structure and technical specifics.
