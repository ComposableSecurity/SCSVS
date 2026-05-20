# C11: Stablecoin

## Control Objective

Stablecoins — collateralized (DAI, USDe, crvUSD), fiat-backed (USDC, USDT), algorithmic (former UST), and hybrid — are systemic infrastructure for DeFi. Their failure modes (depeg, hyperinflation, bad-debt accumulation, frozen collateral) propagate to every protocol that integrates them, and the on-chain stablecoin contract itself is a high-value target.

Ensure that a verified contract satisfies the following high-level requirements:
* The peg mechanism is precisely specified and its failure modes are accounted for,
* Mint and burn authority is strictly controlled and observable,
* Collateral, surplus, and debt are accounted for so the issuer cannot become silently insolvent.

Category "C11" lists requirements related to the stablecoin smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C11.1** | Verify that the peg mechanism (CDP, PSM, algorithmic rebase, redeemable claim) is precisely documented, including the assets, oracles, and incentives that maintain it. |
| **C11.2** | Verify that mint authority is restricted to a documented set of contracts/roles, every mint emits an event, and per-minter mint caps are enforced on-chain. |
| **C11.3** | Verify that burn authority is symmetric with mint authority where appropriate, and that an attacker who acquires a minter role cannot drain reserves through a mint/redeem loop. |
| **C11.4** | Verify that collateral types are constrained to an allowlist that can only be modified through governance with a notice period. |
| **C11.5** | Verify that liquidation thresholds are set strictly below the LTV at which the position becomes unbacked, with a margin that accounts for oracle latency and gas-limited liquidator response. |
| **C11.6** | Verify that the stability fee, savings rate, or interest accrual model is monotonic and cannot be exploited via donations, rounding, or zero-supply edge cases. |
| **C11.7** | Verify that surplus, system debt, and liquidation buffers are reconciled by an invariant test (e.g. total collateral value >= total debt + targeted surplus). |
| **C11.8** | Verify that an emergency shutdown / global settlement procedure exists, is reachable by an authenticated emergency role, and allows holders to redeem at a documented exchange rate. |
| **C11.9** | Verify that a Peg Stability Module (PSM), if present, has fees and caps that prevent draining of one collateral asset for the other under arbitrage. |
| **C11.10** | Verify that algorithmic or partially-collateralized designs document the death-spiral / reflexivity risk and have circuit breakers that pause new mints when the peg deviates beyond a threshold. |
| **C11.11** | Verify that rebasing or yield-bearing collateral is accounted for using share-based balances, and that share-to-asset conversions round in favor of the protocol. |
| **C11.12** | Verify that cross-chain stablecoin deployments (lock-and-mint, burn-and-release, native multichain) reconcile circulating supply with a documented authoritative source and react to bridge incidents without freezing user redemptions on unaffected chains. |
| **C11.13** | Verify that the stablecoin documents which addresses can blocklist, freeze, or claw back balances, that such functions emit events, and that they cannot be exercised through a low-quorum path. |
| **C11.14** | Verify that EIP-2612 `permit` (and ERC-3009 `transferWithAuthorization`, if present) implementations follow chain-ID binding, permit-front-running DoS handling. |
| **C11.15** | Verify that protocol fees and seigniorage flow to a documented recipient and cannot be redirected silently by a privileged role. |
| **C11.16** | Verify that the stablecoin is not assumed to be a stable accounting unit by integrating protocols: its potential to depeg is reflected by oracle-backed pricing rather than a hardcoded `1e18`. |
| **C11.17** | Verify that liquidation auctions (Dutch, batched, on-demand) cannot be denied-of-service by a single keeper, and that auction parameters cannot be changed atomically with the auction itself. |

## References

For more information, see also:

* [MakerDAO Documentation](https://docs.makerdao.com/)
* [Liquity Protocol](https://docs.liquity.org/)
* [Ethena USDe Documentation](https://ethena-labs.gitbook.io/ethena-labs/)
* [Curve crvUSD Whitepaper](https://docs.curve.fi/crvUSD/overview/)
* [Terra/Luna Depeg Post-mortem](https://www.galaxy.com/research/insights/the-luna-and-ust-crash-explained/)
* [USDC Depeg During SVB Collapse](https://www.circle.com/blog/an-update-on-usdc-and-silicon-valley-bank)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
