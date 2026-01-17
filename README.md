# Public Reports

Welcome to the official public reports repository of [d23e.ch](https://d23e.ch). We are a company specializing in blockchain security and research, focusing on identifying vulnerabilities in both design and code.

## Our Services

1. **Research**: We conduct in-depth research on various aspects of blockchain technology, smart contracts, and decentralized finance (DeFi).
2. **Security**: We offer comprehensive security audits and vulnerability assessments for blockchain projects and smart contracts.

## Repository Contents

This repository contains our public disclosures, research papers, and audit reports. Below is a comprehensive table of our work:

| Date | Project/Topic | Type & Document | New Vulnerabilities/New Findings |
|------|---------------|-----------------|--------------------------|
| Jan 2026 | Immunefi | [Audit Report](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2026-01-immunefi-audit.pdf) | See Report |
| Jan 2026 | AVE AI | [Audit Report](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2026-01-ave-ai-audit.pdf) | Critical: unauthenticated v3-style swap callback can drain router ERC20 balances |
| Jul 2025 | Sanction + Tracing | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2025-07-research-sanction.pdf) | Quantifies OFAC sanctions on Tornado Cash; deposits down ~71% but still used in ~78% incidents<br>Limitations: dusting, partial enforcement, obfuscation; proposes impurity scoring/tracking |
| Jul 2025 | A1 | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2025-07-reserach-a1.pdf) | A1 turns LLMs into an execution-validated exploit generator<br>36 contracts; 63% success on VERITE; attacker/defender cost asymmetry |
| Sep 2024 | Amplification Attack | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2024-09-research-amplification-attack.pdf) | Disclosed to bloXroute<br>Disclosed to Eden network |
| Aug 2024 | Aurigami Protocol | [Vulnerability Report](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2024-08-aurigami-protocol-bug-report.pdf) | Critical empty-market rounding issue enables exchange-rate inflation and draining (Compound v2 fork, Aurora)<br>Impacts auUSDC/auUSDT native markets and multiple lending markets |
| Aug 2024 | Ethereum Mempool DoS | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2024-08-research-ethereum-mempool-DoS.pdf) | Disclosed to Ethereum foundation (bug bounty received)<br>Disclosed to Flashbots (bug bounty received)|
| May 2024 | BX Digital | [Audit Report](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2024-05-audit-bx-digital.pdf) | PVE-001 [^2] (Medium): AssetAddress can be variable balance token<br>PVE-002 [^2] (Low): Malicious oracle can manipulate the trade order<br>PVE-003 [^2] (Info): Front-running possibility when the oracle is malicious |
| Aug 2023 | Generalised Front-Running | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2023-08-research-generalised-front-running.pdf) | MassDeposit: Vulnerability in massDeposit() risking $28.58M (ETH) and $759.54K (BSC).<br>Unverified Stake: BSC staking flaw allowing instant profit from unverified assets.<br>Unauthenticated Minting: BSC token flaw enabling unlimited token minting.<br>Unauthenticated Asset Redemption: Contracts on ETH and BSC allowing unrestricted asset redemptions.<br>Faulty Authentication: 8 contracts (ETH/BSC) enabling unauthorized asset transfers. |
| Jun 2023 | SwissBorg | [Audit Report](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2023-06-audit-swissborg.pdf) | 3 potential security vulnerabilities that could compromise system integrity and safety.<br>3 informational findings to improve contract code quality. |
| May 2023 | EPG | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2023-05-research-epg.pdf) | Uniswap + Tokenlon, flaw in token design leads to continous arbitrage opportunities |
| Apr 2023 | SoK DeFi Attacks | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2023-04-sok-defi-attacks.pdf) | First systematization of DeFi attacks |
| Dec 2021 | Quantifying Ethereum MEV | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-12-research-quantifying-ethereum-mev.pdf) | First comprehensive measurement on Ethereum MEV; first generalized front-running algorithm  |
| Oct 2021 | Liquidation | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-10-research-aave-compound-makerdao-dydx-liquidation.pdf) | Aave flaw in liquidation design leads to double liquidation |
| Jun 2021 | DeFiPoser | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-06-defiposer.pdf) | First automated tool for discovering profit-generating DeFi transactions |
| Jun 2021 | A2MM | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-06-a2mm.pdf) | First on-chain aggregator design|
| Mar 2021 | Flashloan | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-03-flashloan.pdf) | First work on DeFi attacks |
| Mar 2021 | Confuzzius | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2021-03-confuzzius.pdf) | First hybrid fuzzer for smart contracts |
| Dec 2019 | Sandwich attack | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2019-12-sandwich.pdf) | Disclosed to Uniswap |
| Oct 2018 | Securify | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2018-10-securify.pdf) | First scalable verifier for Ethereum smart contracts |
| Dec 2016 | Ethereum Eclipse | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2016-12-ethereum-eclipse.pdf) | Exploiting Ethereum's block propagation vulnerabilities to perform eclipse attacks |
| Oct 2016 | PoW Security | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2016-10-pow-security.pdf) | First systematic work quantifying security and performance trade-offs in proof-of-work blockchains |
| Oct 2015 | Bitcoin Delay Propagation | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2015-10-bitcoin-delay-propagation.pdf) | Exposing adversarial block and transaction delays in Bitcoin's network propagation |
| Dec 2014 | Bloom Filters | [Research Paper](https://github.com/Decentralized-Intelligence/PublicSecurityReviews/blob/main/2014-12-bloom-filters.pdf) | Revealing privacy leaks in SPV Bitcoin clients using Bloom filters |


## About Our Work

Our work spans various aspects of blockchain security and research. Through our security disclosures, we help improve the safety of popular blockchain wallets and platforms. Our audit reports demonstrate our commitment to enhancing the security of blockchain projects. The research papers showcase our contributions to advancing knowledge in critical areas such as MEV, front-running, DoS attacks, and DeFi protocol analysis.

For more information about our services or to engage with us, please visit our website at [https://d23e.ch](https://d23e.ch).
