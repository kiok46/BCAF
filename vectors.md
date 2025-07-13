These vector might not create a vulnerability for some contract systems but these are very important to consider.


# Table of Contents

- [Version Control](#version-control)
- [Transaction Structure](#transaction-structure)
- [Reverse Category](#reverse-category)
- [Constructor Parameters](#constructor-parameters)
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


## Version Control

Each transaction version has its own set of rules and changes to the transaction structure. By not explicitly stating the version, there is a risk of unintentional use of the contract, leading to vulnerabilities.

**Version 2** enables timelocks.

`require(tx.version==2)`

Use of timelocks will be enabled when version 2 is enforced.

If the contract is static and the locking bytecode doesn't change on a case-by-case basis, then it's recommended to use a version check for future upgrades.

If the contract uses timelocks, then it should use version 2.

---


## Transaction Structure

Ensure the transaction structure is well defined and enforced, for contracts that do not rely on a strict transaction structure, it should be explicitly stated in the documentation with a defined expected structure instead.

- Defined Inputs and Outputs restrictions:
    ```
    require(tx.inputs.length==x)
    require(tx.outputs.length==y)
    ```

- Any Inputs and Outputs restrictions:
    ```
    require(tx.inputs[x].lockingbytecode == require(tx.outputs[y].lockingbytecode))
    ```

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