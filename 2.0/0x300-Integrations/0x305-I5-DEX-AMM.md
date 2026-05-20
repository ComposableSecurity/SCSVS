# I5: DEX / AMM integration

## Control Objective

DEX/AMM integration — routing swaps through Uniswap V2/V3/V4, Curve, Balancer, or aggregators (1inch, 0x, KyberSwap, Odos, Paraswap) — is among the most common integration patterns and one of the most frequently exploited. The risks are not limited to slippage: callbacks, approval scopes, mid-swap reentrancy, malicious routers, and read-only price reads have each caused major incidents.

Ensure that the integration process satisfies the following high-level requirements:
* Swaps are bounded in slippage and time and do not leak value to back-runners or sandwich attackers,
* Approvals to routers and pools are scoped and revocable,
* Prices read from DEX state are not used as oracles unless explicitly hardened.

Category "I5" lists requirements related to integration with DEX and AMM contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **I5.1** | Verify that every swap call passes a user-meaningful `minAmountOut` and an absolute `deadline`, and that both are propagated to the router/pool call. |
| **I5.2** | Verify that spot prices read from an AMM pool (`getReserves`, `slot0`, `getRate`) are never used as the price oracle for critical business logic. |
| **I5.3** | Verify that the set of routers, factories, and pools the protocol calls is restricted to an allowlist, and that calls to untrusted addresses cannot be routed through the same integration path. |
| **I5.4** | Verify that approvals to routers use `SafeERC20.forceApprove`, are reset to zero after use when the integration is one-shot, or use Permit2 with a scoped allowance. |
| **I5.5** | Verify that multi-hop routes verify each intermediate token against an allowlist and that the protocol cannot be routed through a malicious or unverified token in the middle of the path. |
| **I5.6** | Verify that Uniswap V3 `uniswapV3SwapCallback`, V4 hook callbacks, and Balancer / Curve flash-swap callbacks authenticate that the caller is the expected pool (`msg.sender == expectedPool`) and that the callback context corresponds to a swap the protocol initiated. |
| **I5.7** | Verify that read-only reentrancy via the integrated DEX (e.g. Curve V2 / Balancer LP pricing during a swap) is mitigated by checking the pool's reentrancy guard or by using safe price-fetch helpers. |
| **I5.8** | Verify that quoter contracts (Uniswap V3 `Quoter`, aggregator `quote` endpoints) are not used as on-chain price sources — they are simulation helpers and are manipulable. |
| **I5.9** | Verify that swaps over fee-on-transfer, rebasing, or callback-bearing tokens use a pre/post balance check on the recipient and treat the delta as the realized amount, not the nominal `amountOut`. |
| **I5.10** | Verify that native-ETH versus WETH semantics at the router boundary are correct: the protocol does not lose ETH by passing `msg.value` to a router path that expects WETH (or vice versa). |
| **I5.11** | Verify that aggregator routers (1inch, 0x, KyberSwap, Odos) are integrated with an `expectedReturn` floor enforced by the protocol, not by the aggregator alone (aggregator quote ≠ executed amount). |
| **I5.12** | Verify that the protocol's own swap path cannot be turned into a sandwich vehicle: large internal rebalances, treasury swaps, or oracle-influencing swaps either use a private mempool, batch with other actions atomically, or split into <=N chunks. |
| **I5.13** | Verify that concentrated-liquidity LP positions (Uniswap V3, V4) the protocol manages are reconciled on every operation (fees collected, liquidity recomputed) and that position NFTs are non-transferable or owner-restricted. |
| **I5.14** | Verify that integration with Uniswap V4 hooks treats the hook contract as untrusted unless the protocol controls it: hook return values, dynamic fees, and afterSwap deltas can each be malicious. |
| **I5.15** | Verify that deadlines specified for swaps on fast L2s (Arbitrum, Optimism, Base) are not so large that the swap can be back-run after a sequencer outage; protocol-issued swaps prefer short deadlines or block-number-based deadlines. |
| **I5.16** | Verify that Permit2 signatures used in DEX integration are bound to the specific spender and amount and cannot be replayed by a malicious router that holds the signature. |
| **I5.17** | Verify that the integration treats router upgrades or factory deprecations as a documented incident: a deprecated DEX path cannot silently route through a new, unaudited contract. |

## References

For more information, see also:

* [Uniswap V2 Documentation](https://docs.uniswap.org/contracts/v2/overview)
* [Uniswap V3 Documentation](https://docs.uniswap.org/contracts/v3/overview)
* [Uniswap V4 Documentation](https://docs.uniswapfoundation.org/)
* [Curve Read-Only Reentrancy Advisory](https://chainsecurity.com/heartbreaks-curve-lp-oracles/)
* [Permit2 Documentation](https://github.com/Uniswap/permit2)
* [Aggregator Integration Pitfalls](https://blog.openzeppelin.com/dex-aggregator-issues/)
* [Sandwich Attacks Explained](https://blog.chain.link/sandwich-attack/)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
