# G13: MEV resilience

## Control Objective

Maximal Extractable Value (MEV) — the value that block proposers, builders, sequencers, and searchers can extract by reordering, inserting, or censoring transactions — has become a dominant operational risk for DeFi. Users routinely lose value to sandwich attacks, back-running, just-in-time liquidity, and oracle MEV, and protocols routinely surface attack surface they did not intend to expose to block-construction actors.

Ensure that a verified contract satisfies the following high-level requirements:
* Every user-facing flow identifies whether it is exposed to MEV and what mitigation applies,
* Sensitive state transitions (price updates, liquidations, auctions, governance) cannot be profitably front-run or back-run,
* The protocol does not silently leak value to block-construction actors through avoidable design choices.

Category "G13" lists requirements related to MEV resilience of the smart contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G13.1** | Verify that the threat model enumerates every user-facing entry point exposed to MEV (sandwich, back-running, JIT liquidity, oracle MEV, time-bandit re-orgs) and documents its mitigation. |
| **G13.2** | Verify that swap, mint, and redeem entry points enforce both a user-provided minimum output and an explicit deadline. |
| **G13.3** | Verify that liquidations and auctions cannot be front-run in a way that lets the borrower exit at a worse price than the keeper would accept. |
| **G13.4** | Verify that high-value or privileged actions (governance execution, parameter updates, large redemptions) use commit-reveal, a private mempool, or batched-clearing settlement to remove single-block ordering advantages. |
| **G13.5** | Verify that oracle update transactions cannot be back-run by a transaction in the same block that exploits the new price (e.g. by enforcing a one-block oracle delay or by atomicizing the update with consumers). |
| **G13.6** | Verify that batch auctions and sealed-bid mechanisms use uniform clearing prices so that no participant gains by observing other bids. |
| **G13.7** | Verify that signature-authorized flows (permits, intents, meta-transactions) cannot be front-run by an attacker who copies the signature and submits it first to grief the user. |
| **G13.8** | Verify that LP positions and concentrated-liquidity ranges in the protocol's own AMM cannot be cheaply opened and closed within a single transaction to capture fees without taking inventory risk (JIT mitigation). |
| **G13.9** | Verify that the protocol documents the trust assumptions placed on the L2 sequencer (or proposer-builder pipeline) and what it does if the sequencer becomes adversarial. |
| **G13.10** | Verify that public-mempool transactions submitted by the protocol's own operators (keepers, rebalancers) do not expose strategy secrets that searchers can replicate. |
| **G13.11** | Verify that MEV captured by the protocol (e.g. by an internal solver or order-flow auction) is distributed by a documented policy and cannot be silently redirected by a privileged role. |

## References

For more information, see also:

* [Flashbots: MEV Wiki](https://www.mev.wiki/)
* [Ethereum Foundation: MEV](https://ethereum.org/en/developers/docs/mev/)
* [Just-in-Time Liquidity on Uniswap V3](https://uniswap.org/blog/jit-liquidity)
* [Order Flow, Auctions and Centralisation](https://writings.flashbots.net/order-flow-auctions-and-market-structure)
* [Proposer-Builder Separation](https://ethereum.org/en/roadmap/pbs/)
* [Sandwich Attacks Explained](https://blog.chain.link/sandwich-attack/)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
