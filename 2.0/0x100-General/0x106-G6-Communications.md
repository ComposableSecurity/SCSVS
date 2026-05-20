# G6: Communications

## Control Objective

Communications include the topic of the relations between smart contracts and their libraries.

Ensure that a verified contract satisfies the following high-level requirements:
* The external calls *from* and *to* other contracts have considered abuse cases and are authorized,
* Used libraries are safe and the state-of-the-art security libraries are used.

Category “G6” lists requirements related to the function calls between the verified contracts and other contracts out of the scope of the application.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G6.1** | Verify that libraries that are not part of the application (but the smart contract relies on to operate) are identified. |
| **G6.2** | Verify that *delegatecall* is not used with untrusted contracts (i.e. targets are either immutable, set behind a timelock, or restricted to an allowlist of audited contracts). |
| **G6.4** | Verify that the contract does not check whether the address is a contract using *extcodesize* opcode. |
| **G6.5** | Verify that re-entrancy attack is mitigated by blocking recursive calls from other contracts (e.g., *Checks-Effect-Interactions*, *ReentrancyGuard*). |
| **G6.6** | Verify that the result of low-level function calls (e.g. *send*, *delegatecall*, *call*) from another contracts is checked. |
| **G6.7** | Verify that contract relies on the data provided by the right sender and the contract does not rely on *tx.origin* value. |
| **G6.8** | Verify that contract does not enforce usage of "phantom functions". |
| **G6.9** | Verify that contract does not accept Ether transfers (e.g. via fallback or receive) that cannot be withdrawn. |
| **G6.10** | Verify that read-only reentrancy is mitigated: view functions used by other protocols cannot be queried mid-execution while internal state is inconsistent. |
| **G6.11** | Verify that `try/catch` is used around external calls when failure must not revert the caller, and that out-of-gas, return-bomb, and revert-with-arbitrary-data cases are all handled. |
| **G6.12** | Verify that ERC-2771 trusted-forwarder integrations correctly resolve `_msgSender()` and `_msgData()` and cannot be spoofed by an untrusted forwarder. |
| **G6.13** | Verify that transient-storage-based reentrancy locks (EIP-1153) account for nested calls into the same contract and are cleared even on revert paths. |


## References

For more information, see also:

* [Security Considerations: Sending and Receiving Ether](https://solidity.readthedocs.io/en/v0.5.10/security-considerations.html#sending-and-receiving-ether)
* [DASP 10: Unchecked Return Values For Low Level Calls](https://www.dasp.co/#item-4)
* [SWC-107 Reentrancy](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-107)
* [SWC-112 Delegatecall to Untrusted Callee](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-112)
* [Phantom functions](https://media.dedaub.com/phantom-functions-and-the-billion-dollar-no-op-c56f062ae49f)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
