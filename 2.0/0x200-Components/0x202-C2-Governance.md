# C2: Governance

## Control Objective

If a project contains a Governance smart contract, it is necessary to follow the standard and create a secure contract based on it. Learn from past mistakes that have been identified and have solutions ready.

Ensure that a verified contract satisfies the following high-level requirements:
* Contract follows a tested and stable Governance standard,
* The project is managed by a DAO or a multisig contract,
* Vulnerabilities identified in various Governance implementations have been taken into account during implementation.

Category “C2” lists requirements related to the Governance smart contract as one of the project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C2.1** | Verify that Governance contract follows tested and stable Governance standards. |
| **C2.2** | (DAO) Verify that voting power is sampled at a past snapshot block (e.g. ERC-20Votes checkpoint) so that flash-loaned or borrowed tokens cannot influence votes. |
| **C2.3** | (DAO) Verify that the votes over taking actions by governance are counted correctly, following the documentation.  |
| **C2.4** | (DAO) Verify that the delays associated with the enforcement of actions are consistent with those described in the documentation. |
| **C2.5** | (DAO) Verify that the actions can only be performed after voting is completed (unless explicitly stated otherwise in the documentation).
| **C2.6** | (DAO) Verify that the actions can be performed only following the voting result (voted yes - must be performed, voted no or not obtaining the required number of votes - cannot be performed).
| **C2.7** | Verify that special actions that can be performed only by Governance are marked in the documentation. |
| **C2.8** | Verify that the Governance cannot do more than declared in the documentation. |
| **C2.9** | Verify that the ownership transfer is a 2-step process where the candidate accepts it in a separate transaction and the previous owner does not lose privileges until it is done. |
| **C2.10** | Verify that key operations on the Governance contract can only be performed with the appropriate permissions. Functions in this contract should not be available to general users.|
| **C2.11** | Verify that a process has been established whereby proposals that users will vote on are searched for suspicious functions (e.g., selfdestruct) and they are rejected or proceed with caution. |
| **C2.12** | Verify that a process has been established whereby proposals that users will vote on are audited by external company. |
| **C2.13** | Verify that the protocol treats vote-buying / bribery markets as a documented threat and that critical parameter changes (oracles, upgrade keys, fee receivers) require additional safeguards beyond a single vote (e.g. timelock with veto). |
| **C2.14** | Verify that the governance contract enforces minimum participation (quorum) and that quorum cannot be reached by a single privileged delegate without explicit design intent. |
| **C2.15** | Verify that vote delegation cannot be used to double-count voting power (no transfer-then-delegate-back loops within a single proposal). |
| **C2.16** | Verify that proposal execution is atomic: a single failing action in a multi-call proposal reverts the entire batch. |
| **C2.17** | Verify that quorum/threshold parameters cannot be lowered atomically with a malicious message in the same block (parameter changes are time-locked). |

## References

For more information, see also:

* [Understanding the Tornado Cash Governance Attack](https://composable-security.com/blog/understanding-the-tornado-cash-governance-attack/)
* [Strategies for Secure Governance with Smart Contracts](https://www.youtube.com/watch?v=GbDAmMdmh8Q)
* [MakerDAO Governance Module](https://docs.makerdao.com/smart-contract-modules/governance-module)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
