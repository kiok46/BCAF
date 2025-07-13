# Table of Contents

1. [Version Control](#version-control)
2. [Transaction Structure](#transaction-structure)
3. [Reverse Category](#reverse-category)
4. [Constructor Parameters](#constructor-parameters)

- Stack doesn't end up resulting to true.
- Splitting issues, split at invalid index
- Split based on length
- Split and assignment
- little endian and big endian
- Invalid conversion
- p2sh32 recommended, p2sh20 not recommended
- verify lockingbytecode restrictions OR scriptHash or PKH
- Transaction structure
- Documentation and explaination of each step
- NatSpec style documentation
- Math issues
- UTXO injection
- satoshi value leak
- tokenAmount value leak
- category leak
- capability mutation
- sig issue
- data sig issue
- op_return and it's enforcement of data
- if else nesting and invalid re-assignment
- genesis cofiguration issues
- non enforcement of parameters and reliance on functions.
- timelock and relative lock value issues.
- Reverse category
- Construction of contracts within the code issues, (reversed constructor parameters)
- Future upgrade vulnerabilities, nft commitment or version, lockingbytecode.
- Input from p2sh or p2pkh enforcement
- Unintended burn of token or nft
- deadlock situations due to category or state changes
- Contract size too big, > 1650 bytes
- strict version check of cashscript
- Division by 0
- Missing int to byte casting
- Null fail check in sig
- Enforcements of parameters that are governed by standardness.
- tests coverage
- Explicit defining of intermediate states in case of state storage in nfts
- Typing errors
- Misleading documentation
- Inconsistent coding style
- Unused variables
- Unused code
- Unreachable code
- Variable shadowing
- Sending mutable or minting tokens to p2pkh or unknown p2sh

</br>

## Framework Components

### Core Audit Areas

1. **[Entry Point Analysis](entry-point-analysis.md)** - Identify and analyze all contract entry points
2. **[Input/Output Validation](input-output-validation.md)** - Assess input and output restrictions
3. **[Version Control Checks](version-control-checks.md)** - Review timelocks and version requirements
4. **[Upgrade Vulnerability Assessment](upgrade-vulnerability-assessment.md)** - Evaluate future upgrade risks
5. **[Transaction Input Analysis](transaction-input-analysis.md)** - Analyze input injection capabilities
6. **[UTXO Management](utxo-management.md)** - Detect potential UTXO injections and burns
7. **[Token Security](token-security.md)** - Prevent unintentional burns and mints
8. **[Category Management](category-management.md)** - Check for category leaks and NFT capability changes
9. **[State Management](state-management.md)** - Identify deadlocks and state update issues
10. **[Genesis Configuration](genesis-configuration.md)** - Verify proper contract initialization
11. **[Mathematical Validation](mathematical-validation.md)** - Check for calculation errors
12. **[Length Validation](length-validation.md)** - Implement proper length checks

## Version Control

Each transaction version has its own set of rules and changes to the transaction structure. By not explicitly stating the version, there is a risk of unintentional use of the contract, leading to vulnerabilities.

### Version 1

Does not support timelocks.

### Version 2

Enables timelocks.

`require(tx.version==2)`

Use of timelocks will be enabled when version 2 is enforced.

If the contract is static and the locking bytecode doesn't change on a case-by-case basis, then it's recommended to use a version check for future upgrades.

If the contract uses timelocks, then it should use version 2.

---


## Transaction Structure

Ensure the transaction structure is correctly formed

### Inputs restrictions:

`require(tx.inputs.length==x)`

### Outputs restrictions:

`require(tx.outputs.length==x)`

In case of functions with no strict input/output restrictions, it should be explisitly stated in the function description.

---

## Reverse Category

By default the token category is reversed, to ensure that the category is used correctly in the constructor the inversed should be used either in the genesis or during the usage in the contract.

## Example

```solidity
Contract Foo (bytes category) {
   function bar() {
        require(tx.inputs[0].tokenCategory.reverse() == category);
    }
}
```

OR


```solidity
Contract Foo (bytes reversedCategory) {
   function bar() {
        require(tx.inputs[0].tokenCategory == reversedCategory);
    }
}
```


---


## Constructor Parameters

The parameters are added inversely to the constructor.

In case of `simulated states` or any other case, it must be used in inverse order.