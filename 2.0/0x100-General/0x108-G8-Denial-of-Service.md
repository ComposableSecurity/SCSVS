# G8: Denial of service

## Control Objective

Ensure that a verified contract satisfies the following high-level requirements:
* The contract logic prevents influencing the availability of the contract.

Category “G8” lists requirements related to the possible denial of service of the smart contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G8.1** | Verify that the contract does not iterate over unbound loops.  | 
| **G8.3** | Verify that the business logic does not block its flows when any of the participants are absent forever. | 
| **G8.4** | Verify that the contract logic does not disincentivize users to use contracts (e.g. the cost of the transaction is higher than the profit). | 
| **G8.5** | Verify that expressions of functions *assert* or *require* have a passing variant. | 
| **G8.6** | Verify that if *fallback* function is not callable by anyone, it is not blocking the functionalities of the contract. |
| **G8.7** | Verify that there are no costly operations in a loop. | 
| **G8.8** | Verify that there are no calls to untrusted contracts in a loop. | 
| **G8.9** | Verify that if there is a possibility of suspending the operation of the contract, it is also possible to resume it. | 
| **G8.10** | Verify that if allow lists and deny lists are used, it does not interfere with the normal operation of the system. | 
| **G8.11** | Verify that there is no DoS caused by overflows and underflows. | 
| **G8.12** | Verify that the contract tolerates forced ETH transfers (via `selfdestruct`, `coinbase`, or block reward) without breaking accounting invariants. |
| **G8.13** | Verify that loops over user-influenced arrays (claimers, voters, batched recipients) have a hard cap or pagination that fits within block gas limits on every supported chain. |
| **G8.14** | Verify that signature-replay protections (e.g. EIP-2612 permit nonces) cannot be front-run by an attacker to grief a victim's intended transaction. |
| **G8.15** | Verify that L2 deployments tolerate sequencer downtime: critical user actions (withdrawals, liquidations) must remain reachable via L1 forced inclusion or equivalent. |

## References

For more information, see also:

* [DASP 10: Denial of Service](https://www.dasp.co/#item-5)
* [Gas Limit and Loops](https://solidity.readthedocs.io/en/v0.5.10/security-considerations.html#gas-limit-and-loops)
* [Gas Limit DoS on the Network via Block Stuffing](https://consensys.github.io/smart-contract-best-practices/known_attacks/#gas-limit-dos-on-the-network-via-block-stuffing)
* [DoS with Block Gas Limit](https://consensys.github.io/smart-contract-best-practices/known_attacks/#dos-with-block-gas-limit)
* [SWC-128 DoS With Block Gas Limit](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-128)
* [SWC-113 DoS with Failed Call](https://smartcontractsecurity.github.io/SWC-registry/docs/SWC-113)
* [Uncallable function example](https://github.com/ethereum/EIPs/issues/820#issuecomment-454021564)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**