---
description: The various scoring types of Merkl campaigns
---

# Scoring Types

## What is Scoring?

**Scoring types** define how individual user contributions are measured and converted into reward shares. While [distribution types](distributions.md) determine the overall reward model and budget allocation, scoring types control how each user's activity is scored within that model.

At its core, Merkl computes **time-weighted integrals** of user balances or contributions—essentially tracking how much liquidity or assets each user provided over time.

## Scoring Types Overview

Merkl supports various scoring methods, allowing campaign creators to choose how individual user contributions are measured. Each scoring type applies a different function to the time-weighted integrals, creating distinct reward allocation patterns.

- **Default scoring (1:1 mapping)**: By default, Merkl uses a proportional scoring system where a user's share of rewards equals their share of the total time-weighted contributions. For example, if a user contributed 10% of the total liquidity-time across all participants, they receive 10% of the rewards (in a variable APR campaign).

- **Custom scoring functions**: Campaign creators can apply different scoring functions to transform these integrals before calculating reward shares. Examples include:

  - **Maximum balance cap**: Taking the minimum between a user's time-weighted balance and a predefined cap—this limits how much eligible balance counts toward rewards, preventing excessive concentration.
  - **Logarithmic scoring**: Applying a logarithmic function to the time-weighted integral—this reduces the advantage of large contributors, creating a more equitable distribution where smaller participants receive proportionally higher rewards relative to their contribution size.
