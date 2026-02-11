# ⚡️ Solidity Internals & EVM Research

![Solidity](https://img.shields.io/badge/Solidity-%23363636.svg?style=for-the-badge&logo=solidity&logoColor=white)
![Ethereum](https://img.shields.io/badge/Ethereum-3C3C3D?style=for-the-badge&logo=Ethereum&logoColor=white)
![Security](https://img.shields.io/badge/Security-Research-blue?style=for-the-badge)

**A curated collection of deep dives into smart contract architecture, security patterns, and low-level EVM mechanics.**

> *"To master smart contracts, one must look beyond the syntax and understand the underlying machine."*

---

## 🎯 Objective

This repository serves as a personal knowledge base and research portfolio. The goal is to deconstruct popular libraries and standards to understand:
1.  **Why** they are written that way.
2.  **How** they optimize for Gas.
3.  **What** security vulnerabilities they prevent.

---

## 🗂️ Research Topics

### 🔐 Security Patterns
Deep analysis of security mechanisms used in industry-standard protocols.

| Topic | Description | Status |
| :--- | :--- | :--- |
| **[ReentrancyGuard Internals](./01-openzeppelin-reentrancy/analysis-reentrancy-guard.md)** | Why OpenZeppelin uses `uint256` (1 & 2) instead of `bool`. | ✅ Completed |
| *Timelock Controller* | Analysis of delayed execution patterns. | ⏳ Upcoming |
| *Access Control* | Role-based permission logic breakdown. | ⏳ Upcoming |

### ⛽ Gas Optimization
Techniques to reduce transaction costs on the EVM.

| Topic | Description | Status |
| :--- | :--- | :--- |
| *Custom Errors vs Strings* | Comparison of revert costs. | ⏳ Upcoming |
| *Yul / Assembly* | Writing low-level code for efficiency. | ⏳ Upcoming |

---

## 🛠 Tech Stack

* **Language:** Solidity, Typescript
* **Frameworks:** Foundry, Hardhat
* **Focus:** EVM Opcodes, Storage Layout, Memory Management

---

## 👤 Author

**M. Afif Al Falah**
*Smart Contract Researcher & Blockchain Developer*

Passionate about the intersection of Security and Efficiency in decentralized systems.

* [**X**](https://x.com/a_cipher12340) - Follow my daily insights.
* [**GitHub**](https://github.com/cipherbytea-dot) - Check out my code.

---

## ⚠️ Disclaimer
This repository is for **educational purposes only**. The analyses provided are based on personal research and do not constitute professional security audits.