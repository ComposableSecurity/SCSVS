# C14: Restaking

## Control Objective

Restaking protocols (EigenLayer, Karak, Symbiotic, Babylon for Bitcoin) let users opt the same collateral into multiple Actively Validated Services (AVSs), each with its own slashing conditions. The complexity of overlapping slashing scopes, opt-in semantics, and reward distribution has produced a distinct class of risks not covered by liquid-staking requirements (C7).

Ensure that a verified contract satisfies the following high-level requirements:
* Users opt into each AVS explicitly and can observe the slashing risk they have taken,
* Slashing is bounded, attributable, and only reachable via the conditions the user accepted,
* Withdrawals are subject to a delay sufficient to detect and punish misbehavior.

Category "C14" lists requirements related to restaking smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C14.1** | Verify that a user's collateral is subject to slashing by an AVS only after the user has explicitly opted in to that AVS through an on-chain transaction. |
| **C14.2** | Verify that each AVS's slashing condition is bounded by a documented maximum percentage of the staker's collateral, enforced on-chain. |
| **C14.3** | Verify that withdrawal from a strategy is subject to a delay (escrow window) at least as long as the longest slashing-detection window of any AVS the user is opted into. |
| **C14.4** | Verify that an operator cannot opt a delegator into a new AVS without that delegator's explicit opt-in or without a documented opt-out period. |
| **C14.5** | Verify that strategy share dilution caused by slashing is borne by the slashed staker and not retroactively socialized over withdrawn shares. |
| **C14.6** | Verify that operator registration requires a self-bond and that the bond cannot be withdrawn before the longest slashing window expires. |
| **C14.7** | Verify that AVS reward distribution is computed per-strategy and per-operator with a documented rounding direction, and that rewards cannot be re-routed by a privileged role. |
| **C14.8** | Verify that the strategy whitelist (the set of accepted collateral) can only be modified through governance with a notice period sufficient for stakers to exit. |
| **C14.9** | Verify that delegation transfer is a two-step process and that the previous operator retains slashing liability until the transfer is confirmed. |
| **C14.10** | Verify that overlapping slashing scopes (the same collateral covering multiple AVSs) cannot result in cumulative slashing greater than the collateral itself. |
| **C14.11** | Verify that the slashing dispute / veto window cannot be shortened atomically with a malicious slashing request. |
| **C14.12** | Verify that proof-of-misbehavior submissions (BLS double-sign, fraud proofs, ZK proofs) are validated cryptographically on-chain and not delegated to an off-chain committee without a documented trust model. |
| **C14.13** | Verify that AVS-controlled hooks (pre-slash, post-slash, pre-reward) cannot reenter the restaking core to drain other strategies. |
| **C14.14** | Verify that liquid restaking tokens (LRTs) built on top of restaking meet C7 requirements and additionally surface the AVS exposure of their underlying collateral. |
| **C14.15** | Verify that pause mechanisms can be applied per AVS or per strategy without trapping collateral from unaffected scopes. |
| **C14.16** | Verify that beacon-chain withdrawal credentials are rotatable and that the rotation process does not strand staked ETH between strategies. |
| **C14.17** | Verify that the protocol documents the cross-chain attestation model (which chain mints/burns, where slashing is finalized) when restaking is exposed across rollups or appchains. |

## References

For more information, see also:

* [EigenLayer Whitepaper](https://github.com/Layr-Labs/whitepaper/blob/master/EigenLayer_WhitePaper-90226a722e1cb693fa6cf747f808f057c5fa49b3.pdf)
* [EigenLayer Slashing Design](https://docs.eigenlayer.xyz/eigenlayer/security/slashing)
* [Karak Documentation](https://docs.karak.network/)
* [Symbiotic Documentation](https://docs.symbiotic.fi/)
* [Restaking Risks: A Survey](https://www.galaxy.com/research/insights/eigenlayer-restaking-and-the-future-of-pos-security/)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
