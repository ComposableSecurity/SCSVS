# C10: Lending market

## Control Objective

Lending markets (Aave, Compound, Morpho, Spark, Silo, Euler, etc.) are among the most attacked component classes in DeFi. They couple price oracles, interest-rate models, collateral accounting, liquidation incentives, and flash-loan composability into a single accounting plane, and a single rounding mistake or stale price reads can produce an unbacked debt of the entire market.

Ensure that a verified contract satisfies the following high-level requirements:
* Collateral, debt, and interest accounting are consistent across every state transition,
* Liquidations are timely, profitable for keepers, and resistant to manipulation by borrowers,
* The market cannot be put into an unrecoverable bad-debt state by a single transaction or a single price tick.

Category "C10" lists requirements related to the lending-market smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C10.1** | Verify that interest accrual is executed before any state-changing user operation (supply, withdraw, borrow, repay, liquidate, transfer of debt tokens, transfer of receipt tokens). |
| **C10.2** | Verify that health-factor / collateral-ratio computations round in favor of the protocol (collateral rounds down, debt rounds up). |
| **C10.3** | Verify that the protocol enforces supply caps and borrow caps per asset and that caps cannot be bypassed via flash loans, reentrancy, or transfers of receipt tokens. |
| **C10.4** | Verify that liquidations pay the keeper a bounded incentive and that the incentive cannot be set high enough to drain a healthy position. |
| **C10.5** | Verify that a borrower cannot avoid liquidation by self-liquidating at favorable parameters or by repaying with collateral they themselves price. |
| **C10.6** | Verify that the protocol cannot be left with bad debt that is silently absorbed by future suppliers; bad debt is realized, socialized, or covered by a reserve through a documented mechanism. |
| **C10.7** | Verify that close-factor and liquidation-bonus parameters cannot be changed retroactively against open positions in a single transaction (parameter changes are time-locked or apply only to new positions). |
| **C10.8** | Verify that isolation modes (Aave-style) or per-asset borrow scopes cannot be bypassed by transferring receipt tokens or by using a separate account controlled by the same actor. |
| **C10.9** | Verify that e-mode / correlation-mode parameters (relaxed LTVs for correlated assets) are bounded and cannot be applied to non-correlated assets. |
| **C10.10** | Verify that the interest-rate model is monotonic in utilization and cannot be driven into a pathological state by a donation, by precision loss, or by zero-supply edge cases. |
| **C10.11** | Verify that the first depositor cannot inflate the receipt-token (cToken/aToken) exchange rate by a donation, by leveraging the share-price formula, or by triggering early withdrawal — see C4.12. |
| **C10.12** | Verify that flash-loan fees are charged on entry (or otherwise accrued in a way that cannot be evaded by ordering the repay before fee accrual). |
| **C10.13** | Verify that repay-on-behalf cannot grief a borrower (e.g. by removing them from a beneficial campaign, by forcing closure, or by leaving dust that blocks future operations). |
| **C10.14** | Verify that pause mechanisms can be applied per market without trapping user collateral that is not the target of the pause. |
| **C10.15** | Verify that receipt tokens (cToken/aToken) and debt tokens are non-transferable when transferring would invalidate health-factor invariants of either party. |
| **C10.16** | Verify that liquidation handles tokens with transfer fees, rebasing balances, and blocklist semantics, or that such tokens are explicitly disallowed as collateral or debt. |
| **C10.17** | Verify that governance changes to LTV, liquidation threshold, or oracle source are subject to a notice period long enough for users to deleverage. |
| **C10.18** | Verify that the protocol has a documented procedure to handle oracle pause, oracle freeze, or oracle deprecation without leaving positions liquidable at stale prices. |
| **C10.19** | Verify that fee-receiver and treasury withdrawal paths cannot draw from reserves earmarked for bad-debt coverage. |

## References

For more information, see also:

* [Aave V3 Technical Paper](https://github.com/aave/aave-v3-core/blob/master/techpaper/Aave_V3_Technical_Paper.pdf)
* [Compound V3 Documentation](https://docs.compound.finance/)
* [Morpho Blue Whitepaper](https://github.com/morpho-org/morpho-blue/blob/main/morpho-blue-whitepaper.pdf)
* [How Lending Protocols Get Hacked](https://chainsecurity.com/lending-protocol-attacks-overview/)
* [Mango Markets Exploit Post-mortem](https://blog.mango.markets/mango-exchange-loses-100m-in-exploit-d0b08b8e4ba6)
* [Euler Finance Exploit](https://rekt.news/euler-rekt/)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
