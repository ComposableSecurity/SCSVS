# G7: Arithmetic

## Control Objective

Ensure that a verified contract satisfies the following high-level requirements:
* All mathematical operations and extreme variable values are handled safely and predictably.

Category “G7” lists requirements related to the arithmetic operations of the smart contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G7.2** | Verify that the unchecked code snippets from Solidity 0.8.\* do not introduce integer under/overflows. |
| **G7.3** | Verify that the extreme values (e.g. maximum and minimum values of the variable type) are considered and do change the logic flow of the contract. |
| **G7.4** | Verify that non-strict inequality is used for balance equality. |
| **G7.5** | Verify that there is a correct order of magnitude in the calculations. |
| **G7.6** | Verify that in calculations, multiplication is performed before division for accuracy. |
| **G7.7** | Verify that contract does not assume fixed-point precision and use a multiplier or store both the numerator and denominator. |
| **G7.8** | Verify that rounding uses secure granularity (e.g. seconds instead of days). |
| **G7.9** | Verify that rounding in extreme conditions (e.g. very often) does not cause amplified losses (e.g. lack of interests accrued). Use RayWad library for such calculations. |
| **G7.10** | Verify that share/asset conversions use a `mulDiv`-style full-precision routine (e.g. OpenZeppelin `Math.mulDiv`, PRBMath, Solady `FixedPointMathLib`) to prevent intermediate 256-bit overflow and precision loss. |
| **G7.11** | Verify that rounding direction systematically favors the protocol: shares minted round down, shares burned round up, debt rounds up, collateral rounds down. |
| **G7.12** | Verify that all token amounts are scaled to a common precision before comparison or arithmetic, accounting for tokens with non-18 decimals (USDC=6, GUSD=2, WBTC=8). |
| **G7.13** | Verify that fixed-point exponentiation and logarithm routines are bounded and revert on inputs outside their proven domain. |

## References

For more information, see also:

* [DASP 10: Arithmetic Issues](https://www.dasp.co/#item-3)
* [OpenZeppelin: SafeMath](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol)
* [SWC-101 Integer Overflow and Underflow](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-101)
* [Significant Rounding Errors For Interest Calculation](https://github.com/OpenCoreCH/smart-contract-audits/blob/main/reports/c4/rigor.md#high-significant-rounding-errors-for-interest-calculation)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
