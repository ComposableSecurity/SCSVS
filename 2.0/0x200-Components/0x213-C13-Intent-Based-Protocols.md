# C13: Intent-based and order-flow protocols

## Control Objective

Intent-based protocols (UniswapX, CoW Protocol, 1inch Fusion, Across, ERC-7683 cross-chain intents) shift execution from users to solvers competing to fill signed orders. The on-chain settlement contract becomes the trust anchor: it must guarantee that no solver can underpay a user, replay an order, or extract more than the documented fee.

Ensure that a verified contract satisfies the following high-level requirements:
* User-signed intents bind every economically relevant parameter and cannot be replayed,
* Solvers cannot front-run, partial-fill, or reorder intents to a user's detriment,
* Settlement preserves a strict accounting invariant between what the user signed and what they receive.

Category "C13" lists requirements related to intent-based and order-flow smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C13.1** | Verify that every signed intent uses EIP-712 typed data binding `chainId`, `verifyingContract`, the user, the input/output tokens, the input amount, the minimum output amount, and an absolute deadline. |
| **C13.2** | Verify that the settlement contract enforces the user's minimum output amount on the actual delivered amount, after fees, taxes, and slippage. |
| **C13.3** | Verify that price improvement above the user's signed minimum is returned to the user (or to a documented recipient), never silently captured by the solver. |
| **C13.4** | Verify that order hashes are unique and that the contract rejects already-filled, cancelled, or expired orders. |
| **C13.5** | Verify that partial fills (if supported) preserve the proportionality invariant: the user receives at least `signedMinOut * filledIn / signedIn` after fees. |
| **C13.6** | Verify that order cancellation by the user is honored immediately and that an in-flight fill cannot land after cancellation. |
| **C13.7** | Verify that solver-supplied callbacks (pre-fill, post-fill hooks) cannot drain the settlement contract or other users' approvals. |
| **C13.8** | Verify that Permit2 (or equivalent universal-approval) usage scopes each transfer to the order being filled and cannot be replayed against another order with the same signature. |
| **C13.9** | Verify that solver selection (auction, first-fill, RFQ) does not allow a solver to censor or front-run the user's own broadcast of their intent. |
| **C13.10** | Verify that fee parameters (protocol fee, solver tip) have bounded maxima enforced in code, not just by governance discipline. |
| **C13.11** | Verify that cross-chain intents (ERC-7683 or equivalent) bind both the origin and destination chain IDs into the signed payload, and that destination-side settlement verifies the origin chain finality. |
| **C13.12** | Verify that token allowances granted to the settlement contract cannot be combined with a malicious solver call to spend more than the order's input amount. |
| **C13.13** | Verify that interaction with fee-on-transfer, rebasing, and callback-bearing tokens is documented and handled (or such tokens are excluded from the allowlist). |
| **C13.14** | Verify that the settlement contract emits events sufficient to reconstruct order, solver, fill amount, and fee, enabling off-chain auditing of solver behavior. |
| **C13.15** | Verify that order hashes cannot be forged by hash-collision attacks on `abi.encodePacked` (see G14.9). |
| **C13.16** | Verify that pause / circuit-breaker mechanisms exist for the settlement contract and that they fail safely (in-flight fills revert, signed orders cannot be settled until resumption). |
| **C13.17** | Verify that solver bonds, allowlists, or reputational gates (if used) cannot be unilaterally bypassed and that slashing of misbehaving solvers is enforceable. |

## References

For more information, see also:

* [UniswapX Whitepaper](https://uniswap.org/whitepaper-uniswapx.pdf)
* [CoW Protocol Documentation](https://docs.cow.fi/)
* [ERC-7683: Cross-Chain Intents Standard](https://eips.ethereum.org/EIPS/eip-7683)
* [Across Protocol](https://docs.across.to/)
* [Permit2](https://github.com/Uniswap/permit2)
* [Intent-Centric Architectures](https://www.paradigm.xyz/2023/06/intents)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
