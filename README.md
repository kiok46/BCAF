# Bitcoin Cash Audit Framework (BCAF)

The Bitcoin Cash Audit Framework (BCAF) is a comprehensive standard for auditing smart contracts on the Bitcoin Cash network.

BCAF delivers a structured methodology for identifying security vulnerabilities, operational risks, and adherence to best practices tailored to the BCH ecosystem.

Every contract should be systematically evaluated against a detailed checklist of audit criteria. For each identified vulnerability or failed check, the framework requires clear documentation and reporting, ensuring transparency and actionable remediation guidance.

It is hoped that projects will use this framework as a foundation to write more comprehensive and accurate audit reports.

## Motivation

With the inevitable rise of complex contract structures due to enhanced VM capabilities of Bitcoin Cash, it is important to have a framework that can be used to audit these contracts. Hope is for BCAF to act as a stepping stone or become the go to standard for auditing contracts on Bitcoin Cash.

---

## Audit Report Structure

The following structure is expected to be followed in audit reports based on this framework:

### Table of Contents

- [Summary](#summary)
- [Scope](#scope)
  - [Example Scope Documentation](#example-scope-documentation)
- [System Overview](#system-overview)
  - [What to Document](#what-to-document)
  - [Example System Overview](#example-system-overview)
- [Findings](#findings)
  - [Severity Levels](#severity-levels)
  - [Example Structure](#example-structure)
- [Framework Components](#framework-components)
  - [Core Audit Areas](#core-audit-areas)

## Summary

The Summary table provides a high-level overview of the audit results, including project details, timeline, technology stack, total number of contracts audited, and issue distribution by severity.

- **Project Information**: Fill in the project name, audit timeline, and technology stack
- **Total Contracts**: Specify the total number of smart contracts included in the audit
- **Issue Counting**: Count all findings by severity level and resolution status
- **Resolution Status**: Track resolved (R), partially resolved (PR), and unresolved (U) issues
- **Severity Classification**: Use the severity levels defined in the framework
- **Total Calculation**: Ensure the sum of all severity levels equals the total issues count

**Note**: The summary table should be updated throughout the audit process and finalized before report delivery.


| Field                        | Description                                   | Value                    |
|------------------------------|-----------------------------------------------|-------------------------|
| **Name**                     | Official project name                         | Name                  |
| **Timeline**                 | Audit start and end dates                     | Date range (YYYY-MM-DD) |
| **Languages**                | Primary smart contract language(s)            | Language (comma-separated) |
| **Total Contracts**          | Number of smart contracts audited             | (Total contracts)        |
| **Total Issues**             | Sum of all findings (resolved/partially resolved) | (Total number) (resolved/partially resolved) |
| **Critical Severity Issues** | Issues that could lead to fund loss           | (Total number) (resolved/partially resolved) |
| **High Severity Issues**     | Issues that could cause denial of service     | (Total number) (resolved/partially resolved) |
| **Medium Severity Issues**   | Minor financial risk or degraded functionality | (Total number) (resolved/partially resolved) |
| **Low Severity Issues**      | Non-exploitable but bad practice issues      | (Total number) (resolved/partially resolved) |
| **Notes & Additional Info**  | Informational findings and recommendations    | (Total number) (resolved/partially resolved) |


### Example Summary Table

| Field                        | Value                    |
|------------------------------|-------------------------|
| **Name**                     | BitCANN                  |
| **Timeline**                 | 2025-07-13 to 2025-07-15 |
| **Languages**                | CashScript |
| **Total Contracts**          | 9                        |
| **Total Issues**             | 0  |
| **Critical Severity Issues** | 0  |
| **High Severity Issues**     | 0  |
| **Medium Severity Issues**   | 0  |
| **Low Severity Issues**      | 0  |
| **Notes & Additional Info**  | 0  |

## Scope

The Scope section documents the specific codebase, commit hash, and files that were audited. This provides transparency about what was reviewed and helps stakeholders understand the audit coverage. Specify any limitations or exclusions from the audit.

- **Repository Details**: Specify the full repository name and exact commit hash
- **File Organization**: Use tree structure to show file hierarchy clearly
- **Complete Coverage**: List all files that were reviewed
- **Summary**: Provide a count of total files and directories audited
- **Exclusions**: If any files were excluded, clearly state the reason

**Note**: The scope should be comprehensive and accurate to ensure audit transparency and reproducibility.

#### Example Scope Documentation

We audited the [<span style="text-decoration: underline; color: inherit;">BitCANN/bitcann-contracts</span>](https://github.com/BitCANN/bitcann-contracts) repository at commit [<span style="text-decoration: underline; color: inherit;">a22e234</span>](https://github.com/BitCANN/bitcann-contracts/commit/a22e234843232112da72f23a181d81e8048ffa11).

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

**Note**: There is already a comprehensive list of known ways a contract can become vulnerable. It is encouraged to refer to these existing vulnerability patterns when conducting audits. If a vulnerability pattern is not yet documented in the framework, please create a Pull Request to contribute it.

### Example Structure

If you have findings, organize them like this:

## Critical Severity Issues

### [C-01] Missing Version Control in Timelock Functions

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

## Medium Severity Issues

### [M-01] Insufficient Input Validation

**Description**: Function accepts arbitrary input without proper validation.

**Impact**: Could lead to unexpected behavior or transaction failures.

**Recommendation**: Add proper input validation.

**Update**: Input validation was implemented for all public functions. The fix was merged in [PR #124](https://github.com/project/repo/pull/124). The development team implemented comprehensive input validation across all affected functions.

---

## Low Severity Issues

### [L-01] Missing Events

**Description**: Important state changes don't emit events.

**Impact**: Reduces transparency and makes debugging difficult.

**Recommendation**: Add events for all important state changes.

**Update**: Events were added for all state-changing functions. The improvement was merged in [PR #125](https://github.com/project/repo/pull/125). The development team agreed with the recommendation and added comprehensive event logging.

---

## Informational Issues

### [I-01] Gas Optimization Opportunity

**Description**: Function could be optimized to use less gas.

**Impact**: Higher transaction costs for users.

**Recommendation**: Consider gas optimizations.


## Conclusion

None



## Copyright

This document is placed in the public domain.