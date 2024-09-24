# d23e.ch Research and Security

Welcome to the official repository of [d23e.ch](https://d23e.ch). We are a company specializing in blockchain security and research, focusing on identifying vulnerabilities in both design and code.

## Our Services

1. **Research**: We conduct in-depth research on various aspects of blockchain technology, smart contracts, and decentralized finance (DeFi).
2. **Security**: We offer comprehensive security audits and vulnerability assessments for blockchain projects and smart contracts.

## Repository Contents

This repository contains our public disclosures, research papers, and audit reports. Below is a comprehensive table of our work:

| Date | Project/Topic | Type & Document | New Vulnerabilities/New Findings |
|------|---------------|-----------------|--------------------------|
| Sep 2024 | Amplification Attack | [Research Paper](2024-09-research-amplification-attack.pdf) | Disclosed to bloXroute<br>Disclosed to Eden network |
| Aug 2024 | Ethereum Mempool DoS | [Research Paper](2024-08-research-ethereum-mempool-DoS.pdf) | Disclosed to Ethereum foundation (bug bounty received)<br>Disclosed to Flashbots (bug bounty received)|
| May 2024 | BX Digital | [Audit Report](2024-05-audit-bx-digital.pdf) | PVE-001[^2] (Medium): AssetAddress can be variable balance token<br>PVE-002[^2] (Low): Malicious oracle can manipulate the trade order<br>PVE-003[^2] (Info): Front-running possibility when the oracle is malicious |
| Sep 2023 | **Hidden**[^1] | [Security Disclosure](2023-09-disclosure-project-t.md)[^3] | Centralized risk to steal users' fund |
| Aug 2023 | Role Play Attacks | [Research Paper](2023-08-role-play-attacks.pdf) | |
| Aug 2023 | Generalised Front-Running | [Research Paper](2023-08-research-generalised-front-running.pdf) | MassDeposit: Vulnerability in massDeposit() risking $28.58M (ETH) and $759.54K (BSC).<br>Unverified Stake: BSC staking flaw allowing instant profit from unverified assets.<br>Unauthenticated Minting: BSC token flaw enabling unlimited token minting.<br>Unauthenticated Asset Redemption: Contracts on ETH and BSC allowing unrestricted asset redemptions.<br>Faulty Authentication: 8 contracts (ETH/BSC) enabling unauthorized asset transfers. |
| Jul 2023 | BlockWallet | [Security Disclosure](2023-07-disclosure-blockwallet.md)[^3] | New attack vector: bypassing swap fee |
| Jul 2023 | **Hidden**[^1] | [Security Disclosure](2023-07-disclosure-project-m.md)[^3] | New attack vector: bypassing swap fee |
| Jun 2023 | Huckleberry | [Security Disclosure](2023-06-huckleberry.md) | Critical: Lend bug - A malicious user can steal all funds deposited into the protocol. Possible Loss: All TVL (highest ~$300k in history, now 50k) |
| Jun 2023 | SwissBorg | [Audit Report](2023-06-audit-swissborg.pdf) | 3 potential security vulnerabilities that could compromise system integrity and safety.<br>3 informational findings to improve contract code quality. |
| May 2023 | EPG | [Research Paper](2023-05-research-epg.pdf) | Uniswap + Tokenlon, flaw in token design leads to continous arbitrage opportunities |
| Apr 2023 | SoK DeFi Attacks | [Research Paper](2023-04-sok-defi-attacks.pdf) | |
| Dec 2021 | Quantifying Ethereum MEV | [Research Paper](2021-12-research-quantifying-ethereum-mev.pdf) | |
| Oct 2021 | Liquidation | [Research Paper](2021-10-research-aave-compound-makerdao-dydx-liquidation.pdf) | Aave flaw in liquidation design leads to double liquidation |
| Jun 2021 | DeFiPoser | [Research Paper](2021-06-defiposer.pdf) | |
| Jun 2021 | A2MM | [Research Paper](2021-06-a2mm.pdf) | First on-chain aggregator design|
| Mar 2021 | Flashloan | [Research Paper](2021-03-flashloan.pdf) | |
| Mar 2021 | Confuzzius | [Research Paper](2021-03-confuzzius.pdf) | |
| Dec 2019 | Sandwich attack | [Research Paper](2019-12-sandwich.pdf) | Disclosed to Uniswap |
| Oct 2018 | Securify | [Research Paper](2018-10-securify.pdf) | |
| Dec 2016 | Ethereum Eclipse | [Research Paper](2016-12-ethereum-eclipse.pdf) | |
| Oct 2016 | PoW Security | [Research Paper](2016-10-pow-security.pdf) | |
| Oct 2015 | Bitcoin Delay Propagation | [Research Paper](2015-10-bitcoin-delay-propagation.pdf) | |
| Dec 2014 | Bloom Filters | [Research Paper](2014-12-bloom-filters.pdf) | |

[^1]: Hidden for security reasons, as the issue has not yet been fixed.
[^2]: PVE stands for Potential Vulnerability Exposure.
[^3]: Proof of Concept (PoC) included to demonstrate the feasibility.

## About Our Work

Our work spans various aspects of blockchain security and research. Through our security disclosures, we help improve the safety of popular blockchain wallets and platforms. Our audit reports demonstrate our commitment to enhancing the security of blockchain projects. The research papers showcase our contributions to advancing knowledge in critical areas such as MEV, front-running, DoS attacks, and DeFi protocol analysis.

For more information about our services or to engage with us, please visit our website at [https://d23e.ch](https://d23e.ch).
