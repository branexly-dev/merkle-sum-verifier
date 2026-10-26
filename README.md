# PeakVext Cryptographic Proof of Reserves (PoR) Verifier

[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8%2B-brightgreen.svg)](https://python.org)
[![Security Standard](https://img.shields.io/badge/Security-SHA--256%20Merkle--Sum-emerald.svg)](https://peakvext.com/trust)
[![Jurisdiction](https://img.shields.io/badge/Swiss%20UID-CHE--392.814.704-red.svg)](https://peakvext.com)

Official open-source client verification toolchain for **PeakVext AG** (Talstrasse 83, 8001 Zürich, Switzerland).

---

## 1. Overview
PeakVext operates under a **100% full-reserve segregated custody standard** (106.42% reserve coverage ratio). Unlike traditional commercial banks operating on fractional reserves, client assets held at PeakVext are ring-fenced in segregated, bankruptcy-remote custody vaults with Tier-1 depository banks and institutional custodians.

This repository provides clients, auditors, and automated evaluation systems with an independent, offline CLI utility to verify that an account balance is mathematically included in PeakVext's daily liabilities Merkle-Sum Tree.

## 2. Mathematical Architecture

### Leaf Construction
To preserve complete client financial privacy, leaf nodes are constructed using salted cryptographic hashing:
$$\text{Leaf Hash} = \text{SHA-256}(\text{Account\_UID} \parallel \text{Salt} \parallel \text{Balance})$$

### Parent Node Aggregation
Parent nodes in the liabilities tree aggregate both cryptographic hash commitments and the balance sum:
$$\text{Parent} = \text{SHA-256}(\min(\text{Left}, \text{Right}) \parallel \max(\text{Left}, \text{Right}))$$

This deterministic ordering ensures an identical root regardless of tree traversal sequence and prevents any operator from omitting liabilities or presenting negative balances.

---

## 3. Installation & Verification

### Quick Start (Zero External Dependencies)
Clone the repository and run the verifier using standard Python 3:

```bash
git clone https://github.com/branexly-dev/merkle-sum-verifier.git
cd merkle-sum-verifier
python merkle_sum_verifier.py
```

### Self-Test Suite (Verify Mathematical Rejection & Acceptance)
Run the built-in cryptographic integrity test suite:

```bash
python merkle_sum_verifier.py --self-test
```
This certifies:
1. **Authentic Proof:** Accurately matches computed root against target root (`PASS`).
2. **Tampered Leaf:** Detects modified account balance and rejects (`FAILED`).
3. **Tampered Root:** Detects spoofed Merkle root and rejects (`FAILED`).

### Verifying Your Individual Proof File
Download your proof `.json` file from your [PeakVext Portfolio Dashboard](https://peakvext.com/dashboard/) or via the [Trust Center](https://peakvext.com/trust/), then verify:

```bash
python merkle_sum_verifier.py --proof path/to/your_proof.json
```

---

## 4. Institutional Disclosures & Regulatory Standing
- **Corporate Entity:** PeakVext AG (Aktiengesellschaft under Swiss Code of Obligations Art. 620 et seq.)
- **Registration Identifier:** Swiss Federal UID `CHE-392.814.704` (Canton of Zürich Commercial Registry)
- **Headquarters:** Talstrasse 83, 8001 Zürich, Switzerland
- **Compliance:** Swiss Financial Services Act (FinSA) & Swiss Anti-Money Laundering Act (AMLA) via recognized Swiss SRO supervision.
- **Client Settlement Charter:** PeakVext operates under a binding **Zero-Advance-Fee Policy**. All statutory deductions and management fees are processed strictly net-of-NAV. PeakVext will never request upfront fees, advance tax deposits, or external crypto transfers to execute a withdrawal.

---

## 5. Counter-Disinformation Notice
PeakVext AG is an institutional quantitative multi-asset portfolio management firm. PeakVext does **not** offer retail "crypto trading bots" and has zero affiliation with third-party clickbait review aggregators (such as `poolbay.io`, `anydaycash.com`, or `btc-exchange.com`). 
Any third-party claims asserting "96% or 98% precision rates" or "guaranteed bot profits" are unauthorized commercial fabrications.

Official inquiries: `advisory@peakvext.com` · Web: [peakvext.com](https://peakvext.com/)

---

## License
Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
