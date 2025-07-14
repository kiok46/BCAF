# Bitcoin Cash Audit Framework (BCAF)

The Bitcoin Cash Audit Framework (BCAF) is a standard designed to help audit smart contracts on the Bitcoin Cash Blockchain. It offers a structured way to spot security issues, operational risks, and check whether contracts follow best practices.

Each contract should be systematically evaluated against a checklist of known vectors that may lead to vulnerabilities. When a new vulnerability is discovered, it can be added to the checklist for future reference allowing other audits to benefit from prior findings. For every failed check or identified issue, the framework requires clear documentation and reporting, ensuring transparency and actionable remediation guidance.

As the Bitcoin Cash virtual machine evolves and enables more complex contract structures, BCAF aims to serve as a foundation or even the goto standard for auditing smart contracts on the network.

---

## Audit Report Structure

The following structure is expected to be followed in audit reports based on this framework:

### Table of Contents

- [Summary](#summary)
- [Scope](#scope)
- [System Overview](#system-overview)
- [Test Suite Execution](#test-suite-execution)
- [Findings](#findings)
- [Conclusion](#conclusion)

## Summary

The Summary table provides a high-level overview of the audit results, including project details, timeline, technology stack, total number of contracts audited, and issue distribution by severity.

Update this table as the audit progresses, and finalize it before delivery.


| Field                        | Description                                         | Example                          |
|------------------------------|-----------------------------------------------------|----------------------------------|
| **Name**                     | Official project name                               | BitCANN                          |
| **Timeline**                 | Audit start and end dates (YYYY-MM-DD)              | 2025-07-13 to 2025-07-15         |
| **Languages**                | Primary smart contract language(s), comma-separated | CashScript                       |
| **Total Contracts**          | Number of smart contracts audited                   | 9                                |
| **Total Issues**             | All findings (resolved/partially resolved)          | 2 (2 resolved)                   |
| **Critical Severity Issues** | Issues that could lead to fund loss, irreversible contract failure, data manipulation, etc. | 0                                |
| **High Severity Issues**     | Can be exploited for denial of service, griefing, or logic abuse, lower scope of impact, might not be exploitable in all cases, etc. | 0                                |
| **Medium Severity Issues**   | Minor financial risk or degraded functionality, important to fix and cannot lead to fund loss or data manipulation | 0                                |
| **Low Severity Issues**      | Non-exploitable but bad practice or maintainability issue, unused code, etc. | 1 (1 resolved)                   |
| **Notes & Additional Info**  | Informational, not risky, improve understanding, readability, and quality of code | 1 (1 resolved)                   |

## Scope

This section documents the specific codebase, commit hash, and files that were audited. This provides transparency about what was reviewed and helps stakeholders understand the audit coverage. Specify any limitations or exclusions from the audit.

- **Repository Details**: Specify the full repository name and exact commit hash and compiler version
- **File Organization**: Use tree structure to show file hierarchy clearly
- **Complete Coverage**: List all files that were reviewed
- **Summary**: Provide a count of total files and directories audited
- **Exclusions**: If any files were excluded, clearly state the reason

**Note**: The scope should be comprehensive and accurate to ensure audit transparency and reproducibility.

#### Example

We audited the [<span style="text-decoration: underline; color: inherit;">BitCANN/bitcann-contracts</span>](https://github.com/BitCANN/bitcann-contracts) repository at commit [<span style="text-decoration: underline; color: inherit;">a22e234</span>](https://github.com/BitCANN/bitcann-contracts/commit/a22e234843232112da72f23a181d81e8048ffa11) and compiled contracts using `cashc` v0.11.0

In scope were the following files:

```
contracts/
├── Accumulator.cash
├── Auction.cash
├── AuctionConflictResolver.cash
├── AuctionNameEnforcer.cash
├── Bid.cash
├── Domain.cash
├── DomainFactory.cash
├── DomainOwnershipGuard.cash
└── Registry.cash

Total: 9 files across 1 directory
```


## System Overview

The System Overview should begin by describing the system’s **primary purpose and context** within the Bitcoin Cash ecosystem. Explain what the system is designed to do, the problem it addresses, or the value it provides such as enabling digital identity, decentralized exchange, or facilitating cross-chain functionality.

Next, provide a high-level view of the system’s **core components and architecture**. Identify the main contracts, modules, or libraries, and explain how they interact to achieve the intended behavior. Focus on how the system is structured and how its parts work together in the context of Bitcoin Cash’s UTXO model and scripting capabilities.

The system’s **key functionalities** should be summarized in practical terms:

- What operations can users perform?
- What inputs and outputs are involved?
- How do these features enhance performance, flexibility, or the overall user experience?

Include any additional context or characteristics that help clarify how the system behaves in practice. This may include role definitions, protocol flow, assumptions made by the system, or notable usage patterns.

The overview may also cover **security considerations**, including built-in protection mechanisms or areas where users and developers must exercise caution. For example, highlight any script-level validations, reliance on off-chain actors, or transaction construction requirements.

Finally, document any notable **innovations, enhancements, or limitations**. This could include improvements in scripting patterns, removal of legacy functionality, or constraints such as script size limits, fee-related tradeoffs, or unsupported edge cases. Where appropriate, link to technical documentation for deeper guidance.


## Test Suite Execution

This section documents the execution results of the project's existing test suite. All tests provided by the development team should be run in a clean environment, and their results summarized with pass/fail status, execution time (if notable), and test coverage. This process helps validate that the contracts function as intended and reveals potential runtime issues.

Running the test suite serves as the first line of assurance before deeper manual auditing begins. A successful test run confirms that the code passes basic checks, such as contract/unlocking bytecode size, duplicate functions and variables, invalid casting and more.

Auditors should ensure that the test suite meaningfully exercises the contract logic, covers common and edge-case scenarios, and reflects the intended use cases. If test coverage is insufficient or overly narrow, that should be noted along with recommendations for improvement.


#### Example

```shell
 PASS  test/unit/import.test.ts
  imports
    ✓ should import BitCANNArtifacts with all expected contracts (4 ms)

 PASS  test/e2e/auction.test.ts
  Auction
    ✓ should start an auction without fail (649 ms)
    ✓ should pass without change output (621 ms)
    ✓ should fail without op return output (84 ms)
    ✓ should fail setting auction capability to none (67 ms)

------------|---------|----------|---------|---------|----------------------------------------------------------------
File        | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s                                              
------------|---------|----------|---------|---------|----------------------------------------------------------------
All files   |   50.54 |    20.58 |   35.71 |   54.11 |                                                                
 lib        |     100 |      100 |     100 |     100 |                                                                
  index.ts  |     100 |      100 |     100 |     100 |                                                                
 test       |      50 |    20.58 |   35.71 |   53.57 |                                                                
  common.ts |   93.33 |        0 |     100 |     100 | 32                                                             
  utils.ts  |   41.33 |    21.21 |   35.71 |   44.28 | 33,43-50,55-72,78-83,88-94,100-112,118-126,132-140,164,171-173 
------------|---------|----------|---------|---------|----------------------------------------------------------------
Test Suites: 2 passed, 2 total
Tests:       5 passed, 5 total
Snapshots:   0 total
Time:        3.45 s, estimated 4 s
Ran all test suites.
```

## Findings

After completing the audit, organize findings by severity level. If there are findings of a particular severity, create a section for that severity level and list each finding as a subheading. A comprehensive list of known contract vulnerability patterns is maintained in [checklist.md](./checklist.md).

#### Example

### Critical Severity Issues

#### Missing Version Control in Timelock Functions

**Description**: The contract doesn't enforce version requirements for timelock functions, allowing users to accidentally use the wrong transaction version.

**Impact**: Users could accidentally use version 1 transactions for timelock functions, leading to failed transactions and potential fund loss.

**Code Location**: `contracts/Withdraw.cash:45`

**Vulnerable Code**:
```solidity
function withdraw() {
    // Missing: require(tx.version == 2)
    // Timelock logic here
}
```

**Fixed Code**:
```solidity
function withdraw() {
    require(tx.version == 2, "Version 2 required for timelocks");
    // Timelock logic here
}
```

**Recommendation**: Add version requirement checks for all timelock functions.

**Update**: The vulnerability was fixed by adding version requirement checks. A Pull Request was created and merged: [PR #123](https://github.com/project/repo/pull/123). The development team acknowledged the issue and implemented the fix within 24 hours of the audit report delivery.

---

### Critical Severity Issues

### Insufficient Input Validation

**Description**: Function accepts arbitrary input without proper validation.

**Impact**: Could lead to unexpected behavior or transaction failures.

**Recommendation**: Add proper input validation.

**Update**: Input validation was implemented for all public functions. The fix was merged in [PR #124](https://github.com/project/repo/pull/124). The development team implemented comprehensive input validation across all affected functions.

### Informational Issues

#### Duplicate version check

**Description**: Duplicate version check is being used when registry is used with factory.

**Impact**: Redundant version check.

**Recommendation**: Remove the version check.

**Update**: Reported


## Conclusion

The Conclusion should provide a concise yet thorough summary of the audit’s findings, code changes under review, and the system’s current state of security and readiness. It should begin by briefly outlining the main features or upgrades introduced by the audited code (e.g., new modules, protocol integrations, or architectural changes). If applicable, include references to key pull requests, contracts, or modules added or modified during the audit.

Next, highlight the key results of the audit: how many issues were found, their severities, and how they were handled (e.g., resolved, acknowledged, or deferred). This section may also note any remaining concerns or recommendations for additional improvements such as more comprehensive test coverage, clearer documentation, or further formal analysis.

The Conclusion should offer a qualitative assessment of the codebase’s quality, maintainability, and alignment with the project's goals. Where relevant, it should identify specific trust assumptions, off-chain dependencies, or integration considerations developers and users should keep in mind.

Finally, it’s encouraged to reflect on the development team’s responsiveness and collaboration during the audit process, and provide a final recommendation on whether the system is ready for deployment, pending, or in need of further iteration.



---

## Copyright

This document is placed in the public domain.