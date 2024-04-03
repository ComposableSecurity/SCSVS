# C8: Liquidity pool

## Control Objective

If a project manages a liquidity pool, it is necessary to follow the standard and create secure contracts based on it. Learn from past mistakes that have been identified and have solutions ready.

Ensure that a verified contract satisfies the following high-level requirements:
* Contracts follow the best security practices for liquidity pools,
* Rewards for users are distributed accordingly to promises,
* Potential threats related to liquidity pools are taken into consideration.

Category “C8” lists requirements related to the liquidity pool smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C8.1** | Verify whether sending funds to the pool's address before its creation will not disturb its proper functioning. |
| **C8.2** | Verify that if there are several pools, the *poolId* is present and validated correctly. |
| **C8.3** | Verify that the tokens used by the pools are from the predefined accepted list of addresses determined by the DAO. |
| **C8.4** | Verify that the user is informed and aware about the risks associated with the use of a pool that allows the use of any tokens. |
| **C8.5** | Verify that there are sufficient checks to ensure liquidity availability. |


## References
For more information, see also:
* [How do LIQUIDITY POOLS work?](https://www.youtube.com/watch?v=cizLhxSKrAc)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**