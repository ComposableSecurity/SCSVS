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

### Security Requirements for Integration with LayerZero 

| # | Description |
| --- | --- |
| **I4.LZ.1** | Verify that the message sender (msg.sender) of OFT cross-transfer is allowed to transfer tokens on the owner's behalf. |
| **I4.LZ.2** | Verify that the message sender (msg.sender) of ONFT cross-transfer is approved to transfer NFT token or is its owner. |
| **I4.LZ.3** | Verify that the `_from` address (in `_debitFrom` function) specified in the ONFT cross-transfer is the owner of transfered NFT token. |
| **I4.LZ.4** | Verify that the inbound and outbound block confirmation numbers have been set with care, assuming the possible reorg depths on particular chains. |
| **I4.LZ.5** | Verify that the function calls on the destination chain are try-catched and retryable, unless you are builing a blocking application on purpose. |
| **I4.LZ.6** | Verify that the UA's owner (ideally multi-sig) is able to call `forceResumeReceive` on the LZ endpoint to unblock the cross-chain channel and protect from DoS. |
| **I4.LZ.7** | Verify that the LayerZero cross-chain call parameters are not hardcoded and can be passed in the function call. That includes: `zroPaymentAddress`, `adapterParams`, `refundAddress`, `useZro`. |
| **I4.LZ.8** | Verify that the User Application implementation is able to call the configuration functions on the LZ endpoint (covered if UA inherits from `LzApp`). |
| **I4.LZ.9** | Verify that the User Application has been configured by the protocol or the protocol excplictly accepts the default configuration. |
| **I4.LZ.10** | Verify that the protocol uses production-ready examples of code prepared by LayerZero team for specific usages (e.g. `OFT`, `ONFT`, `LzApp`, `NonBlockingLzApp`). |
| **I4.LZ.11** | Verify that the protocol uses `solidity-examples` package to import LayerZero contracts and does not copy-paste them directly from the repository. |
| **I4.LZ.12** | Verify that the token briding protocol uses `OFT` contract for new tokens and `ProxyOFT` contract for existing tokens. |
| **I4.LZ.13** | Verify that, when using `LzApp`, the implementation uses `_lzSend` function instead of calling `lzEnpoint.send` function directly. |
| **I4.LZ.14** | Verify that, when using `LzApp`, the implementation does not check the same requirements again (using `require` function). |




## References

For more information, see also:
* [LayerZero Integration Checklist](https://layerzero.gitbook.io/docs/evm-guides/layerzero-integration-checklist) 
* [LayerZero: Best Practice](https://layerzero.gitbook.io/docs/evm-guides/best-practice)
* [LayerZero from First Principles](https://medium.com/@PrimordialAA/layerzero-from-first-principles-c2393eb1718d)
* [LayerZero: OFTV2](https://layerzero.gitbook.io/docs/evm-guides/layerzero-omnichain-contracts/oft/oftv2)
* [LayerZero: UA Configuration Lock](https://layerzero.gitbook.io/docs/evm-guides/ua-custom-configuration/ua-configuration-lock)
* [LayerZero: solidity-examples package](https://www.npmjs.com/package/@layerzerolabs/solidity-examples)
