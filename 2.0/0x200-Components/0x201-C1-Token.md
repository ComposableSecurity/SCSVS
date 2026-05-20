# C1: Token

## Control Objective

If a project contains a Token smart contract, it is necessary to follow the standard and create a secure contract based on it. Learn from past mistakes that have been identified and have solutions ready.

Ensure that a verified contract satisfies the following high-level requirements:
* Contract follows a tested and stable Token standard,
* Vulnerabilities identified in various Token implementations have been taken into account during implementation.

Category “C1” lists requirements related to the Token smart contract as one of the project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C1.1** | Verify that the token contract follows tested and stable Token standards. |
| **C1.2** | Verify that the security considerations of the standard used are known and handled by the team.  |
| **C1.3** | Verify that the total token supply matches the documentation. |
| **C1.4** | Verify that the number of minted and burned tokens does not disturb the operation of the system specified in the documentation. |
| **C1.5** | Verify that if the token contains a fee, its maximum value is predetermined and matches the documentation. |
| **C1.6** | Verify that the transfer business logic is consistent, especially when re-sending tokens to the same address (*from == to*). |
| **C1.7** | Verify that if the implemented functions include external calls, they are handled correctly and do not introduce unsafe external calls to the system. |
| **C1.9** | Verify that the token does not have double entry point and does not track user balances in more than one smart contract. |
| **C1.10** | Verify that approval changes are exposed via a safe pattern (`forceApprove` / set-to-zero-then-set) or via EIP-2612 `permit`, and that integrators are documented on the chosen approach. |
| **C1.11** | Verify that EIP-2612 `permit` (and any other signature-based approval) binds the signature to chain ID and contract address through an EIP-712 domain separator that is recomputed on chain-ID changes (fork protection). |
| **C1.12** | Verify that the token contract supports EIP-1271 signatures, or that integrators are notified it does not, when smart-account holders are an expected user base. |
| **C1.13** | Verify that the token does not introduce hooks (ERC-777 `tokensReceived`, ERC-1363 callbacks, transfer-fee callbacks) that expose holders to read-only or cross-function reentrancy. |

## References

For more information, see also:

* [Token Implementation Best Practice](https://consensys.github.io/smart-contract-best-practices/tokens/)
* [iToken Duplication Incident Report](https://bzx.network/blog/incident)
* [The Dangers of Token Integration](https://www.youtube.com/watch?v=6GaCt_lM_ak)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
