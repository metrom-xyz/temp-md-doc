---
description: >-
  Make sure you're on - metrom.xyz - When in doubt, reach out - The team is here
  to help
---

# Metrom Tech

### This section covers the tech details about Metrom and its services

Metrom is a powerful tool designed for decentralized exchanges (DEXes), particularly those using concentrated liquidity AMMs, to incentivize liquidity providers (LPs) by launching dedicated campaigns. These campaigns are aimed at optimizing liquidity distribution while rewarding LPs for their participation based on predefined conditions.

## Campaign Creation

Campaigns in Metrom allow users to incentivize liquidity provisioning in specific pools by defining key parameters such as:

• **Targeted pool:** The liquidity pool where incentives will be distributed.

• **Running period:** The timeframe during which rewards will be earned.

• **Rewards**: Up to 5 different reward types can be specified, such as tokens.

• **Reward distribution method:** Creators can tailor how rewards are distributed, either through a pre-approved list (allowlist/blocklist) or based on Key Performance Indicators (KPIs) that measure the performance of LPs.\


## Backend Operations

Once a campaign is launched, Metrom’s backend takes over. It continuously monitors the on-chain activity of the targeted liquidity pool and processes relevant liquidity events like `MINT`, `BURN` and `SWAP`. Metrom computes the reward distribution based on these events off-chain,.

A Merkle tree is generated from the rewards distribution list, which is then securely stored:

• Leaves are stored on IPFS, and

• Root is pushed on-chain.

This approach allows eligible LPs to claim their rewards by providing a proof of inclusion to the Metrom smart contract.

Rewards are allocated to active LPs based on their proportional liquidity contribution to the specified pool.

For KPI based rewards, the campaign owner will be able to recover unmet KPI rewards.

## System Architecture

Metrom’s backend consists of three main service binaries and a shared library:

• **Metronome**: The core service, responsible for indexing campaigns, processing on-chain events, and assigning rewards at specified intervals. It also takes regular snapshots of reward assignments, constructs a Merkle tree, and ensures the root is published on-chain.

• **API**: A warp-based service that pulls data from subgraphs and the Metrom database to provide protocol-related insights to external applications.

• **KPI Tracker**: This service measures relevant KPIs such as total value locked (TVL) in liquidity pools, which is crucial for calculating rewards in KPI-based campaigns.

## Metronome Service

Metronome operates by indexing campaigns into a database and processing them at regular intervals. Processing involves analyzing on-chain events from the referenced liquidity pool(s) to determine the active LPs and their liquidity positions, which dictate reward assignments.

Additionally, Metronome performs snapshotting at configurable intervals. This process converts the reward assignments into a Merkle tree structure, which is then:

• Published to IPFS (and S3 for internal redundancy),

• The Merkle root is posted on-chain for verification.

The tree format is simple and stores account addresses, reward token information, and the assigned reward amounts. Example:

```
[
  {
    "account": "0xacf3b45850904a1d59128073e8706be1381934de",
    "token": "0xaf88d065e77c8cc2239327c5edb3a432268e5831",
    "amount": "10029178"
  },
  {
    "account": "0x0834f84a2e8fc6f778a055944b8e05f1b4837192",
    "token": "0xaf88d065e77c8cc2239327c5edb3a432268e5831",
    "amount": "0"
  },
  {
    "account": "0xc36442b4a4522e871399cd717abdd847ab11fe88",
    "token": "0xaf88d065e77c8cc2239327c5edb3a432268e5831",
    "amount": "1318844"
  }
]
```

The Metronome service also updates the on-chain reward token rate for whitelisted tokens at specified intervals, ensuring up-to-date conditions for reward distribution.

## API service

The API offers a streamlined interface for interacting with Metrom, providing data from the subgraph and the Metronome database. Users can query details about active campaigns, reward distribution, and other protocol-related information.

## KPI Tracker

This service tracks the on-chain performance of liquidity pools, including TVL and other KPIs that are crucial for KPI-based campaigns. These metrics are stored and analyzed to determine the performance of campaigns and the distribution of rewards based on KPI achievements.

***

**Follow us on** [**X**](https://twitter.com/metromxyz)**, or Join us on** [**Discord**](https://discord.com/invite/S2kBEAGWbM) **or** [**Telegram**](https://t.me/metrom\_xyz)&#x20;
