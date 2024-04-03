# C7: Liquid staking

## Control Objective

If a project manages liquid staking, it is necessary to follow the standard and create secure contracts based on it. Learn from past mistakes that have been identified and have solutions ready.

Ensure that a verified contract satisfies the following high-level requirements:
* Contracts follow best security practices for liquid staking,
* Rewards for users are distributed accordingly to promises,
* Potential threats related to liquid staking are taken into consideration.

Category “C7” lists requirements related to the liquid staking smart contracts as  project components.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **C7.1** | Verify that the rewards mechanism for staked tokens is implemented correctly, and that rewards are distributed fairly and proportionally to all stakers. |
| **C7.2** | Verify that all rewards and fee values are in accordance with the documentation. |
| **C7.3** | Verify that the rewards mechanism is protected against manipulation or exploitation by malicious actors, such as those who attempt to stake large amounts of tokens to gain an unfair advantage. |
| **C7.4** | Verify that during the first stake (when the pool is empty) the appropriate amount of tokens are received by the user. |
| **C7.5** | Verify that the staking protocol has appropriate controls in place to prevent unauthorized access to the staked coins and the system's infrastructure. |
| **C7.6** | Verify that the protocol has a mechanism for detecting and preventing any attempts at "double-staking" or "stake grinding". |
| **C7.7** | Verify that there is a mechanism in place to detect and prevent any attempts at "staking pool front-running," where a malicious staking pool operator can manipulate the rewards mechanism to their advantage. |
| **C7.8** | Verify that the user bond is sufficient to cover protocol risks. |
| **C7.9** | Verify that the rewards mechanism is protected against "slashing attacks" where a malicious actor attempts to cause other stakers to lose their rewards by attempting to vote for multiple validators at once. |
| **C7.10** | Verify that the protocol has a way to handle the situation where a validator goes offline or becomes unresponsive. |
| **C7.11** | Verify that the protocol has a way to handle the situation where a validator is found to be compromised and their private keys are leaked. |
| **C7.12** | Verify that withdrawal requests cannot be canceled or that canceling them does not affect APR protocols. |
| **C7.13** | Verify that withdrawals are not constructed in a way that encourages the staker to stay in the withdrawal queue even though they do not want to withdraw. |
| **C7.14** | Verify that the redemption rate at which a request is fulfilled cannot be better than the redemption rate on the withdrawal request creation (if it’s a multi-step process). |
| **C7.15** | Verify that if the positive rebases affect requests in the queue, it is only to the point where rebases compensate for previously accrued penalties and don’t push the redemption rate higher than it was at the moment of the withdrawal request’s creation. |
| **C7.16** | Verify that there is an established mechanism for selecting and ejecting validators in the event of unstake to recover the 32 ETH entrusted to the operator. |
| **C7.17** | Verify that there is a mechanism to rotate withdrawal credentials. |


## References
For more information, see also:
* [Staking withdrawals](https://ethereum.org/pl/staking/withdrawals/)
* [EIP-4895: Beacon chain push withdrawals as operations](https://eips.ethereum.org/EIPS/eip-4895)
* [Withdrawals landscape](https://hackmd.io/XGrVcicVTm-_IRcIUXPbnQ?view#How-Ethereum-rewards-withdrawals-penalties-amp-slashing-work)
* [How Do Ethereum Withdrawals Work? All You Need To Know](https://www.youtube.com/watch?v=RwwU3P9n3uo)
* [Distributed validator specs](https://github.com/ethereum/distributed-validator-specs)
* [Distributed Validators: Improving Staking No Matter Your ETH Balance](https://www.youtube.com/watch?v=zSt6McTVNVE)
* [MAXIMAL EXTRACTABLE VALUE (MEV)](https://ethereum.org/en/developers/docs/mev/)
* [Proof of Stake FAQ](https://vitalik.ca/general/2017/12/31/pos_faq.html#how-does-validator-selection-work-and-what-is-stake-grinding)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**