# G11: Code clarity

## Control Objective

Ensure that a verified contract satisfies the following high-level requirements:
* The code is clear and easy to read,
* There are no unwanted or unused parts of the code,
* Variable names are in line with good practices,
* The functions being used are secure.

Category “G11” lists requirements related to the code clarity of the smart contracts.

## Security Verification Requirements

| # | Description |
| --- | --- |
| **G11.1** | Verify that the logic is clear and modularized in multiple simple contracts and functions. | 
| **G11.2** | Verify that there is a description in the form of at least 1-2 short sentences of what the contract is for at the beginning of the contract. | 
| **G11.3** | Verify that if the system uses a ready-made implementation of the contract, it has been marked in the comment. If it contains changes from the original, those have been specified. |
| **G11.4** | Verify that the inheritance order is considered for contracts that use multiple inheritances and shadow functions.  | 
| **G11.5** | Verify that the contract uses existing and tested code (e.g. token contract or mechanisms like *ownable*) instead of implementing its own. | 
| **G11.6** | Verify that the same rules for variable naming are followed throughout all the contracts (e.g. use the same variable name for the same object). | 
| **G11.7** | Verify that variables with similar names are not used. | 
| **G11.8** | Verify that the functions which specify a return type return the value. | 
| **G11.9** | Verify that all functions and variables are used. Unused ones should be removed. | 
| **G11.10** | Verify that the custom errors in *if* statement are used instead of the *require* statement. | 
| **G11.11** | Verify that the *assert* function is used to test for internal errors only. | 
| **G11.12** | Verify that assembly code is used only if necessary. |
| **G11.13** | Verify that only functions that are supposed to accept ether are marked as payable. | 


## References

For more information, see also:

* [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html)
* [Solidity: Security Considerations & Recommendations](https://solidity.readthedocs.io/en/v0.5.10/security-considerations.html#recommendations)

## Smart contract audit

Request an audit of your project by SCSVS authors.
**[Contact a specialist](https://composable-security.com/contact/).**