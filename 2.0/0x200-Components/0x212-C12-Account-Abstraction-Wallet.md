# C12: Account-abstraction wallet (ERC-4337 / ERC-7579 / EIP-7702)

## Control Objective

Account-abstraction wallets — ERC-4337 smart accounts, ERC-7579 modular accounts, and EIP-7702 delegated EOAs — have become a primary user-facing surface. They introduce two new attack planes: the validation step (which must be deterministic, bounded, and side-effect-free), and the module/plugin system (which routes arbitrary calls under the account's authority).

Ensure that a verified contract satisfies the following high-level requirements:
* The validation phase is deterministic, gas-bounded, and free of inappropriate side effects,
* Modules, plugins, paymasters, and session keys have strictly defined scopes and revocation paths,
* The account cannot be drained by a malicious bundler, paymaster, or module.

Category "C12" lists requirements related to account-abstraction wallet smart contracts as project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C12.1** | Verify that `validateUserOp` is deterministic, free of external calls that depend on mutable global state, and complies with ERC-4337's validation-phase storage-access rules so that bundlers do not face griefing during simulation. |
| **C12.2** | Verify that the EntryPoint address used by the account is hardcoded or pinned at deployment, and that an attacker cannot redirect the account to a malicious EntryPoint. |
| **C12.3** | Verify that nonce management (sequential or 2D-key per ERC-4337) prevents replay and ordering attacks across bundlers. |
| **C12.4** | Verify that signature aggregation (if used) cannot produce a valid aggregated signature for a UserOp that no signer authorized. |
| **C12.5** | Verify that `validatePaymasterUserOp` rejects UserOps that would exhaust the paymaster deposit beyond the agreed limit, and that paymaster postOp handles reverts of the inner call without losing track of debt. |
| **C12.6** | Verify that session keys are scoped (target contract, selector, value cap, expiry, usage cap) and that scope enforcement happens inside `validateUserOp`, not only off-chain. |
| **C12.7** | Verify that module / plugin installation is gated by the account owner with the same authority required for asset transfer, and that installation emits an event. |
| **C12.8** | Verify that installed modules cannot escalate their own privileges (e.g. install other modules, change ownership, modify the upgrade target) unless that authority was explicitly granted at install time. |
| **C12.9** | Verify that the account's `execute` and `executeBatch` entrypoints can only be invoked by the EntryPoint or the account itself, never by an arbitrary module without explicit routing. |
| **C12.10** | Verify that EIP-1271 `isValidSignature` is implemented and that it respects every signer-side restriction (expiry, scope, usage cap) imposed elsewhere on the account. |
| **C12.11** | Verify that social-recovery threshold parameters can only be changed through the recovery process itself or by the current owner, not through a low-quorum guardian path that creates a re-entrancy through the recovery contract. |
| **C12.12** | Verify that ownership / signer transfer is a two-step operation and that the previous signer set retains authority until the transfer is confirmed. |
| **C12.13** | Verify that EIP-7702 delegation set on the account (or assumed by the account) is taken into account: existing storage and existing code semantics must be consistent with the delegated implementation. |
| **C12.14** | Verify that the account does not silently accept arbitrary calldata in fallback paths that bypass the modular execution router. |
| **C12.15** | Verify that the account handles ETH and ERC-20 / ERC-721 / ERC-1155 receipt hooks without enabling reentrancy into module installation, owner change, or asset withdrawal. |
| **C12.16** | Verify that upgradeability (proxy pattern) on the account, if present, is gated by the same authority that controls the account and is subject to a timelock or guardian veto. |
| **C12.17** | Verify that paymaster sponsorship rules cannot be abused to drain the paymaster through dust UserOps (per-sender rate limits, per-target allowlists). |
| **C12.18** | Verify that bundler-supplied data (preVerificationGas, callGas, verificationGas) is treated as untrusted and the account enforces its own bounds. |

## References

For more information, see also:

* [ERC-4337: Account Abstraction Using Alt Mempool](https://eips.ethereum.org/EIPS/eip-4337)
* [ERC-7579: Minimal Modular Smart Accounts](https://eips.ethereum.org/EIPS/eip-7579)
* [EIP-7702: Set EOA account code](https://eips.ethereum.org/EIPS/eip-7702)
* [ERC-4337 Validation Rules](https://eips.ethereum.org/EIPS/eip-7562)
* [Safe Modular Smart Account Architecture](https://docs.safe.global/advanced/smart-account-overview)
* [ZeroDev / Kernel Account Architecture](https://docs.zerodev.app/sdk/core-api/intro)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
