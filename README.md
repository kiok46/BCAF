# Bitcoin Cash Audit Framework (BCAF)

The Bitcoin Cash Audit Framework (BCAF) is a standard designed to help audit smart contracts on the Bitcoin Cash Blockchain. It offers a structured way to spot security issues, operational risks, and check whether contracts follow best practices.

Each contract should be systematically evaluated against a checklist of known vectors that may lead to vulnerabilities. When a new vulnerability is discovered, it can be added to the checklist for future reference—allowing other audits to benefit from prior findings. For every failed check or identified issue, the framework requires clear documentation and reporting, ensuring transparency and actionable remediation guidance.

As the Bitcoin Cash virtual machine evolves and enables more complex contract structures, BCAF aims to serve as a foundation or even the goto standard for auditing smart contracts on the network.

---

## Audit Report Structure

The following structure is expected to be followed in audit reports based on this framework:

### Table of Contents

- [Summary](#summary)
- [Scope](#scope)
- [System Overview](#system-overview)
- [Findings](#findings)
  - [Severity Levels](#severity-levels)
- [Conclusion](#conclusion)

## Summary

The Summary table provides a high-level overview of the audit results, including project details, timeline, technology stack, total number of contracts audited, and issue distribution by severity.

- **Project Information**: Name, audit timeline, and technology stack
- **Total Contracts**: Number of contracts reviewed
- **Issue Counting**: Count by severity and resolution
- **Resolution Status**: Track resolved (R), partially resolved (PR), and unresolved (U) issues
- **Severity Classification**: Severity should be assessed in context of the specific contract and project
- **Total Calculation**: Must equal the full issue count

Update this table as the audit progresses, and finalize it before delivery.


| Field                        | Description                                         | Example                          |
|------------------------------|-----------------------------------------------------|----------------------------------|
| **Name**                     | Official project name                               | BitCANN                          |
| **Timeline**                 | Audit start and end dates (YYYY-MM-DD)              | 2025-07-13 to 2025-07-15         |
| **Languages**                | Primary smart contract language(s), comma-separated | CashScript                       |
| **Total Contracts**          | Number of smart contracts audited                   | 9                                |
| **Total Issues**             | All findings (resolved/partially resolved)          | 2 (2 resolved)                   |
| **Critical Severity Issues** | Issues that could lead to fund loss                 | 0                                |
| **High Severity Issues**     | Issues that could cause denial of service           | 0                                |
| **Medium Severity Issues**   | Minor financial risk or degraded functionality      | 0                                |
| **Low Severity Issues**      | Non-exploitable but bad practice issues             | 1 (1 resolved)                   |
| **Notes & Additional Info**  | Informational findings and recommendations          | 1 (1 resolved)                   |

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

Document the system's purpose, architecture, and key functionalities. Focus on what the system does, how it works, and what security implications to look for.

#### What to Document

- **Purpose**: What problem does this system solve?
- **Architecture**: How do the contracts interact with each other?
- **Key Functions**: What are the main operations users can perform?
- **Privileged Roles**: Who has special permissions?
- **Asset Flows**: How do funds, tokens, or other assets move through the system?
- **External Dependencies**: What external systems does it rely on?


## Findings

After completing the audit, organize findings by severity level. If there are findings of a particular severity, create a section for that severity level and list each finding as a subheading.

### Severity Levels

- **Critical**: Leads to fund loss, irreversible contract failure
- **High**: Can be exploited for denial of service or logic abuse  
- **Medium**: Minor financial risk or degraded functionality
- **Low**: Non-exploitable but bad practice or maintainability issue
- **Info**: Informational, not risky

**Note**: A comprehensive list of known contract vulnerability patterns is maintained in [vectors.md](./vectors.md).

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

### Medium Severity Issues

### Insufficient Input Validation

**Description**: Function accepts arbitrary input without proper validation.

**Impact**: Could lead to unexpected behavior or transaction failures.

**Recommendation**: Add proper input validation.

**Update**: Input validation was implemented for all public functions. The fix was merged in [PR #124](https://github.com/project/repo/pull/124). The development team implemented comprehensive input validation across all affected functions.

### Low Severity Issues

### Missing Events

**Description**: Important state changes don't emit events.

**Impact**: Reduces transparency and makes debugging difficult.

**Recommendation**: Add events for all important state changes.

**Update**: Events were added for all state-changing functions. The improvement was merged in [PR #125](https://github.com/project/repo/pull/125). The development team agreed with the recommendation and added comprehensive event logging.

### Informational Issues

#### Gas Optimization Opportunity

**Description**: Function could be optimized to use less gas.

**Impact**: Higher transaction costs for users.

**Recommendation**: Consider gas optimizations.


## Conclusion

None



## Copyright

This document is placed in the public domain.