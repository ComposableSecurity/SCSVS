# G14: Cryptography and signatures

## Control Objective

Cryptographic constructions — signatures, hashes, Merkle proofs, and verifier circuits — are the foundation of authentication and authorization in smart contracts. Even small mistakes (a missing chain ID, a non-canonical signature, an `abi.encodePacked` hash collision, an unbounded `ecrecover` return) have repeatedly resulted in critical exploits.

Ensure that a verified contract satisfies the following high-level requirements:
* Every off-chain signed payload is bound to the contract, the chain, the signer, and a single execution,
* Signature verification handles malleability, contract signers, and edge-case return values,
* Hashes and Merkle proofs are built from canonical encodings and verified with sound primitives.

Category "G14" lists requirements related to the cryptographic constructions used by the smart contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G14.1** | Verify that every signed payload uses EIP-712 typed structured data with a domain separator binding `name`, `version`, `chainId`, and `verifyingContract`. |
| **G14.2** | Verify that the domain separator is recomputed whenever `block.chainid` changes, to protect against post-fork replay (cached separators must include a chain-ID check). |
| **G14.3** | Verify that every signed payload includes a signer nonce (monotonic or unordered) and an absolute expiry timestamp, and that the contract rejects expired or replayed payloads. |
| **G14.4** | Verify that signature verification rejects malleable signatures by enforcing `s` in the lower half-order range (EIP-2) and `v` ∈ {27, 28}. |
| **G14.5** | Verify that `ecrecover` returning `address(0)` is treated as a failure, never as a valid signature on the zero address. |
| **G14.6** | Verify that signature verification supports EIP-1271 for contract signers, using OpenZeppelin's `SignatureChecker` or an equivalent that handles both EOA and contract paths safely. |
| **G14.7** | Verify that EIP-1271 verifier calls have a bounded gas budget and treat unexpected revert data and return values as verification failure. |
| **G14.8** | Verify that EIP-2098 compact signatures (if accepted) are normalized before verification and cannot be replayed as a separate 65-byte signature. |
| **G14.9** | Verify that hashes used for authentication are built with `abi.encode` (or canonical type-hashed EIP-712 encoding); `abi.encodePacked` is only used when every argument is fixed-length or when collision-resistance is irrelevant. |
| **G14.10** | Verify that Merkle-proof verification uses sorted-pair hashing (or a leaf/inner-node domain separator) so that an inner node cannot be replayed as a leaf (second-preimage attack). |
| **G14.11** | Verify that Merkle leaves are double-hashed or otherwise domain-separated from inner nodes (e.g. OpenZeppelin's `MerkleProof` convention). |
| **G14.12** | Verify that BLS, BN254, and BLS12-381 precompile calls validate inputs are on-curve and in the correct subgroup before use. |
| **G14.13** | Verify that ZK-proof verifiers bind every public input that the application logic depends on (caller, chain, nonce, amount, recipient) and that no critical input is left implicit. |
| **G14.14** | Verify that verification keys for ZK circuits are immutable or updatable only behind a timelocked governance process, and that the trusted setup (if any) is documented. |
| **G14.15** | Verify that hash preimages and signed payloads cannot be ambiguous between two distinct meanings (e.g. a `bytes` field followed by a variable-length field must be length-prefixed or wrapped in EIP-712 struct hashing). |
| **G14.16** | Verify that randomness used in cryptographic operations (e.g. nonces, salts) is not derived from predictable on-chain sources alone — see G9. |

## References

For more information, see also:

* [EIP-712: Typed structured data hashing and signing](https://eips.ethereum.org/EIPS/eip-712)
* [EIP-1271: Standard signature validation method for contracts](https://eips.ethereum.org/EIPS/eip-1271)
* [EIP-2098: Compact signature representation](https://eips.ethereum.org/EIPS/eip-2098)
* [EIP-2: Signature malleability](https://eips.ethereum.org/EIPS/eip-2)
* [OpenZeppelin SignatureChecker](https://docs.openzeppelin.com/contracts/5.x/api/utils#SignatureChecker)
* [OpenZeppelin MerkleProof](https://docs.openzeppelin.com/contracts/5.x/api/utils#MerkleProof)
* [Hash collisions with abi.encodePacked](https://docs.soliditylang.org/en/latest/abi-spec.html#non-standard-packed-mode)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**
