# I4: Cross-Chain

## Control Objective

If a project integrates with external cross-chain protocol (e.g. LayerZero), it is necessary to take into account their security considerations and best practices.

Ensure that a verified contract satisfies the following high-level requirements:
* Contract follows the security best practices of asynchronous communication between different chains,
* Contract takes into consideration security assumptions from specific messaging protocols.
* Vulnerabilities identified in various integrations with bridges have been taken into account during implementation.

Category “I4” lists requirements related to the smart contracts that make calls to or are called by the bridges' contracts and other contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **I4.1** | Verify that the security checklist provided by the bridge developers team is covered. |
| **I4.2** | Verify that cross-chain call waits for specific number of confirmations before it is transferred to the destination chain and confirmed there. |
| **I4.3** | Verify that the protocol uses production-ready examples of code prepared by the bridge developers team. |
| **I4.4** | Verify that the payload (or other kind of cross-chained message) is encoded securely, using `abi.encode`. Use custom encoding (`abi.encodePacked`) only if you need deep optimization and you know what you are doing. |
| **I4.5** | Verify that the sizes of the destination addresses used in the cross-chained message are verified on the source chain. |
| **I4.6** | Verify that the cross-chain transfer to zero address will revert if the token uses OpenZeppelin's ERC20 on destination chain, because it reverts on mints to zero address. |
| **I4.7** | Verify that cross-chain message handlers are idempotent: a duplicated delivery cannot mint, transfer, or update state twice. |
| **I4.8** | Verify that cross-chain integrations explicitly document and verify the source chain ID and source contract address of every inbound message; the off-chain transport (LayerZero, CCIP, Axelar, Wormhole, Hyperlane) is not relied upon as the sole authority. |
| **I4.9** | Verify that cross-chain integrations apply per-message and per-time-window rate limits / circuit breakers on the destination side. |
| **I4.10** | Verify that cross-chain integrations handle message-execution failure with a documented recovery path (replay, refund, manual quarantine) and that this path cannot itself be abused to double-execute. |
| **I4.11** | Verify that gas/fee parameters for outbound cross-chain messages are configurable per call, not hardcoded, and that refund addresses are passed through securely. |
| **I4.12** | Verify that Chainlink CCIP integrations enable the Risk Management Network "off-switch" awareness and handle the `curseCall` / paused-lane state without locking user funds. |
| **I4.13** | Verify that Wormhole/Axelar/Hyperlane verifications check both the emitter chain ID and emitter address, and that the VAA/message hash cannot be replayed. |

### Security Requirements for Integration with LayerZero

| # | Description |
| --- | --- |
| **I4.LZ.1** | Verify that the message sender (msg.sender) of OFT cross-transfer is allowed to transfer tokens on the owner's behalf. |
| **I4.LZ.2** | Verify that the message sender (msg.sender) of ONFT cross-transfer is approved to transfer NFT token or is its owner. |
| **I4.LZ.3** | Verify that the `_from` address (in `_debitFrom` function) specified in the ONFT cross-transfer is the owner of transfered NFT token. |
| **I4.LZ.4** | Verify that the inbound and outbound block confirmation numbers have been set with care, assuming the possible reorg depths on particular chains. |
| **I4.LZ.7** | Verify that the LayerZero cross-chain call parameters are not hardcoded and can be passed in the function call. That includes: `zroPaymentAddress`, `adapterParams`, `refundAddress`, `useZro`. |
| **I4.LZ.9** | Verify that the User Application has been configured by the protocol or the protocol excplictly accepts the default configuration. |
| **I4.LZ.15** | Verify that the integration inherits from the official `OApp` / `OFT` / `OFTAdapter` base contracts from `@layerzerolabs/lz-evm-oapp-v2` and does not re-implement endpoint plumbing manually. |
| **I4.LZ.16** | Verify that the protocol explicitly sets its send/receive library and its DVN/Executor configuration on the endpoint, rather than relying on LayerZero defaults. |
| **I4.LZ.17** | Verify that the protocol's DVN set has a threshold of at least two independent DVNs and that no single DVN can unilaterally deliver a message. |
| **I4.LZ.18** | Verify that `_lzReceive` overrides validate `_origin.srcEid` and `_origin.sender` against the protocol's own peer mapping before any state change. |
| **I4.LZ.19** | Verify that `setPeer` and configuration calls on `OApp` are restricted to a multisig/timelock and emit events. |
| **I4.LZ.20** | Verify that `lzReceive` execution gas is sized using `enforcedOptions` / `combineOptions` and cannot be exhausted by a malicious composer. |
| **I4.LZ.21** | Verify that `lzCompose` composer integrations validate the source OApp address and message origin before processing. |

## References

For more information, see also:
* [LayerZero Integration Checklist](https://layerzero.gitbook.io/docs/evm-guides/layerzero-integration-checklist)
* [LayerZero: Best Practice](https://layerzero.gitbook.io/docs/evm-guides/best-practice)
* [LayerZero from First Principles](https://medium.com/@PrimordialAA/layerzero-from-first-principles-c2393eb1718d)
* [LayerZero: OFTV2](https://layerzero.gitbook.io/docs/evm-guides/layerzero-omnichain-contracts/oft/oftv2)
* [LayerZero: UA Configuration Lock](https://layerzero.gitbook.io/docs/evm-guides/ua-custom-configuration/ua-configuration-lock)
* [LayerZero: solidity-examples package](https://www.npmjs.com/package/@layerzerolabs/solidity-examples)
