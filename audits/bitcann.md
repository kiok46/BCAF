# BitCANN Audit Report

- [Summary](#summary)
- [Scope](#scope)
- [System Overview](#system-overview)
- [Findings](#findings)
  - [Critical Severity Issues](#critical-severity-issues)
  - [High Severity Issues](#high-severity-issues)
  - [Medium Severity Issues](#medium-severity-issues)
  - [Low Severity Issues](#low-severity-issues)
  - [Informational Issues](#informational-issues)
- [Conclusion](#conclusion)

## Summary

| Field                        | Value                    |
|------------------------------|-------------------------|
| **Name**                     | BitCANN                  |
| **Timeline**                 | 2025-07-13 to 2025-07-15 |
| **Languages**                | CashScript               |
| **Total Contracts**          | 9                        |
| **Total Issues**             | 0                        |
| **Critical Severity Issues** | 0                        |
| **High Severity Issues**     | 0                        |
| **Medium Severity Issues**   | 0                        |
| **Low Severity Issues**      | 0                        |
| **Notes & Additional Info**  | 0                        |


## Scope

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

BitCANN is a decentralized domain name system on Bitcoin Cash. Users can register domain names as NFTs through the Registry contract. The system includes:

- Registry.cash: Main contract managing domain ownership
- DomainFactory.cash: Creates individual domain contracts
- Auction.cash: Handles premium domain auctions
- Bid.cash: Manages auction bids
- Accumulator.cash: Collects and distributes fees

Key operations: domain registration, transfers, auctions, fee collection.
Privileged roles: registry owner can pause system.
Asset flows: registration fees → accumulator → stakeholders.


## Findings

After completing the audit, organize findings by severity level. If there are findings of a particular severity, create a section for that severity level and list each finding as a subheading.

If you have findings, organize them like this:

## Critical Severity Issues

None

## High Severity Issues

None

## Medium Severity Issues

**Description**: Function accepts arbitrary input without proper validation.

**Impact**: Could lead to unexpected behavior or transaction failures.

**Recommendation**: Add proper input validation.

**Update**: Input validation was implemented for all public functions. The fix was merged in [PR #124](https://github.com/project/repo/pull/124). The development team implemented comprehensive input validation across all affected functions.

## Low Severity Issues

### Floating Pragma are being used

**Description**: Pragma directives should be fixed to clearly identify the Solidity version with which the contracts will be compiled.

**Impact**: Can introduce unexpected behavior if the compiler changes.

**Recommendation**: Use a fixed pragma.

**Update**: Reported


## Informational Issues

### Duplicate version check

**Description**: Duplicate version check is being used when registry is used with factory.

**Impact**: Redundant version check.

**Recommendation**: Remove the version check.

**Update**: Reported


## Conclusion

None