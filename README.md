<p align="center">
  <img src="https://img.shields.io/badge/Ethereum-L1_Mainnet-3C3C3D?style=for-the-badge&logo=ethereum&logoColor=white" alt="Ethereum" />
  <img src="https://img.shields.io/badge/ERC--3643-Compliant-00e5ff?style=for-the-badge" alt="ERC-3643" />
  <img src="https://img.shields.io/badge/ZK--Proofs-Circom-c8a84b?style=for-the-badge" alt="ZK-Proofs" />
  <img src="https://img.shields.io/badge/Solidity-0.8.20-363636?style=for-the-badge&logo=solidity&logoColor=white" alt="Solidity" />
  <img src="https://img.shields.io/badge/SDVOSB-Certified-bf5af2?style=for-the-badge" alt="SDVOSB" />
</p>

<h1 align="center">TRADEPROTO.AI</h1>

<h3 align="center">Institutional Intelligence Infrastructure for Global Commodity Trade Finance</h3>

<p align="center">
  Accelerating Capital Velocity through <strong>Verified Trust</strong>, Institutional <strong>RWA Tokenization</strong>, and <strong>Programmable Finance</strong>.
</p>

<p align="center">
  <a href="https://tradeproto.ai">tradeproto.ai</a> &nbsp;&bull;&nbsp;
  <a href="https://tradeproto.ai/dragon-seal.html">Dragon Seal</a> &nbsp;&bull;&nbsp;
  <a href="https://greenhorizoninnovations.com">Green Horizon Innovation</a>
</p>

---

## Platform Overview

TradeProto is a vertically integrated trade finance platform that combines **zero-knowledge proof verified compliance**, **ERC-3643 security token infrastructure**, **automated SBLC settlement**, and **oracle-driven commodity pricing** into a single institutional-grade system.

Every data point is cryptographically provable. Every transaction is ZK-verified. Every query result is tamper-proof. No trust required -- only mathematics.

### Core Capabilities

| Capability | Description |
|---|---|
| **ZK-Proof Compliance** | Circom circuits verify KYC, SGS inspection, maritime B/L, and biometric identity without revealing confidential data |
| **ERC-3643 Tokenization** | Regulated security tokens with on-chain identity registry and modular compliance -- only verified participants can hold or transfer |
| **Automated SBLC Settlement** | Standby Letters of Credit tokenized and settled atomically via smart contracts |
| **Oracle-Driven Pricing** | Multi-source price feeds with staleness detection and deviation circuit breakers |
| **Dragon Seal Identity** | Soulbound NFT identity verification with .dragon file format |
| **Proof of SQL** | Space and Time decentralized data warehouse -- every query result is cryptographically verified |

---

## Architecture

```
                         TRADEPROTO SYSTEM ARCHITECTURE
  ============================================================================

  +---------------------+       +---------------------+       +------------------+
  |                     |       |                     |       |                  |
  |   CLIENT DASHBOARD  | ----> |   ZK CIRCUITS       | ----> |  SMART CONTRACTS |
  |                     |       |                     |       |                  |
  |  - KYC Submission   |       |  - KYC Verifier     |       |  - TRNT Token    |
  |  - Document Upload  |       |  - SGS Inspector    |       |  - IdentityReg   |
  |  - Trade Execution  |       |  - Maritime B/L     |       |  - Compliance    |
  |  - Dragon Seal      |       |  - Biometric ID     |       |  - DragonSeal    |
  |                     |       |                     |       |                  |
  +---------------------+       +---------------------+       +------------------+
           |                                                          |
           |                                                          |
           v                                                          v
  +---------------------+       +---------------------+       +------------------+
  |                     |       |                     |       |                  |
  |   KYC EXTRACTION    | ----> |   ORACLE ROUTER     | ----> |   SETTLEMENT     |
  |   AGENT             |       |                     |       |   ENGINE         |
  |                     |       |  - Price Feeds       |       |                  |
  |  - Gemini 2.5 Flash |       |  - Maritime AIS     |       |  - SBLC Atomic   |
  |  - 12-Section Bank  |       |  - SGS Verification |       |  - Escrow Release|
  |    Template         |       |  - Staleness Check  |       |  - B/L Transfer  |
  |  - Auto-extraction  |       |  - Circuit Breakers |       |  - Dragon Attest |
  |                     |       |                     |       |                  |
  +---------------------+       +---------------------+       +------------------+
                                         |
                                         v
                                +---------------------+
                                |                     |
                                |   SPACE AND TIME    |
                                |   (Proof of SQL)    |
                                |                     |
                                |  - Clients DB       |
                                |  - Documents DB     |
                                |  - Seals DB         |
                                |  - Attestations DB  |
                                |  - Merkle Roots     |
                                |                     |
                                +---------------------+
```

---

## The Quantum Trinity Matrix

Three platforms sharing one cryptographic backbone -- verified identities, tokenized assets, ZK-proven data, and AI agent intelligence flowing seamlessly across the ecosystem.

### TradeProto -- Commodity Trading

The mission-critical infrastructure for global energy trade. Bridges petroleum markets with banking liquidity using ZK-verified compliance, automated SBLC tokenization, and real-time oracle integration for atomic settlement.

**Products:** EN590 | JET-A1 | Crude Oil | LNG | D6

> [Full documentation: docs/trade-flow.md](docs/trade-flow.md)

### WealthProto -- DeFi Liquidity & SBLC Monetization

The Liquidity Layer for Global Energy Infrastructure. Transforms infrastructure bonds and Standby Letters of Credit into programmable collateral, enabling energy leaders to access global capital markets with full transparency and verified compliance.

**Capabilities:** SBLC NFTs | DeFi Vault | RWA Yield

> [Full documentation: docs/wealthproto.md](docs/wealthproto.md)

### TELPAI-QUANTUM -- Geophysical Exploration

Quantum Multi-Agent Geophysical & Deep Space Exploration Framework. 4D holographic hyper-scaling with sedimentary residual magnetic field modeling, satellite data linking, and quantum agent orchestration for real-time subsurface anomaly detection and RWA tokenization.

**Capabilities:** SQUID Magnetometry | 4D Holographic | Magnetics | Seismic

> [Full documentation: docs/telpai-quantum.md](docs/telpai-quantum.md)

---

## Dragon Seal -- Soulbound Identity Verification

Traditional PDF stamps and rubber seals are trivially forged. In commodity trading, a single forged seal on a KYC, SPA, or FCO can enable millions in fraudulent transactions.

**Dragon Seal** is a Soulbound ERC-721 NFT -- one per identity, non-transferable -- that cryptographically binds a participant's biometric composite hash, holographic seal, and digital signature to an on-chain identity. Every document in the trade lifecycle is attested against the Dragon Seal with an immutable timestamp.

### The `.dragon` File Format

A self-contained verification package bundling identity artifacts, attestation history, and on-chain proofs into a single exportable file that any counterparty can independently verify.

```json
{
  "format": "dragon",
  "magic":  "DRGN",
  "identity": {
    "biometricHash": "sha256:a7f3b2e1c9d4...",
    "sealHash":      "sha256:3e8f1a2b5c9d...",
    "signatureHash": "sha256:7d4e2f1a8b3c..."
  },
  "attestations": [
    { "type": "KYC", "hash": "sha256:b2e1...", "tx": "0x4f2a..." },
    { "type": "SPA", "hash": "sha256:c9d4...", "tx": "0x8b1c..." },
    { "type": "FCO", "hash": "sha256:e5f6...", "tx": "0xa3d7..." }
  ],
  "seal": {
    "tokenId": 42,
    "chain": "ethereum:1",
    "contract": "0x7B2c...DragonSeal"
  }
}
```

> [Full specification: docs/dragon-seal.md](docs/dragon-seal.md)

---

## Technology Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Blockchain** | Ethereum L1 Mainnet | Maximum security, institutional credibility, Merkle root anchoring |
| **Smart Contracts** | Solidity 0.8.20, OpenZeppelin | TRNT Token, IdentityRegistry, ModularCompliance, DragonSeal |
| **Token Standard** | ERC-3643 (T-REX) | Regulated security tokens with on-chain compliance gating |
| **Privacy** | Circom ZK Circuits | Zero-knowledge proofs for KYC, SGS, B/L, biometric verification |
| **Database** | Space and Time (Proof of SQL) | Decentralized data warehouse with cryptographically verified queries |
| **Identity** | ERC-721 Soulbound (Dragon Seal) | Non-transferable identity NFTs with biometric hash binding |
| **Oracles** | Custom Oracle Router | Multi-source price feeds, staleness detection, deviation circuit breakers |
| **AI / Extraction** | Gemini 2.5 Flash | KYC extraction agent with 12-section bank template auto-fill |
| **Agent Framework** | QM2ARL / ENGRAM | Multi-agent reinforcement learning for geophysical data and trade intelligence |

---

## OFAC Compliance Framework

TradeProto implements a comprehensive OFAC compliance framework for energy commodity transactions, including:

- **General License** and **Specific License** classification
- Transaction flow architecture for Compliant crude and refined products
- CIF (Cost, Insurance, Freight) structuring through Rotterdam and Houston
- Required documentation chain (KYC, CIS, SPA, FCO, LOI, B/L, SGS)
- Non-US banking corridor considerations
- Automated sanctions screening integration

> [Full framework: docs/ofac-compliance.md](docs/ofac-compliance.md)

---

## DLT Banking Integration Targets

TradeProto is designed to interface with institutional DLT banking platforms:

| Institution | Platform | Integration Focus |
|---|---|---|
| **HSBC** | Orion | Tokenized deposits, trade finance settlement |
| **JP Morgan** | Kinexys (fka Onyx) | Cross-border payments, repo transactions |
| **Standard Chartered** | SC Ventures / Libeara | Tokenized fund distribution, trade finance |
| **DBS** | DBS Digital Exchange | Security token issuance, digital custody |

---

## Repository Structure

```
tradeproto-public/
  README.md                    # This file
  LICENSE                      # Proprietary license
  docs/
    architecture.md            # System architecture deep-dive
    dragon-seal.md             # Dragon Seal NFT specification
    ofac-compliance.md         # OFAC compliance framework
    wealthproto.md             # WealthProto platform documentation
    telpai-quantum.md          # TELPAI-QUANTUM platform documentation
    trade-flow.md              # Complete trade lifecycle
```

---

## Links

| Resource | URL |
|---|---|
| **Platform** | [https://tradeproto.ai](https://tradeproto.ai) |
| **Dragon Seal** | [https://tradeproto.ai/dragon-seal.html](https://tradeproto.ai/dragon-seal.html) |
| **Green Horizon Innovation** | [https://greenhorizoninnovations.com](https://greenhorizoninnovations.com) |

---

<p align="center">
  <strong>Built by Green Horizon Innovation LLC (SDVOSB)</strong><br/>
  <sub>Service-Disabled Veteran-Owned Small Business</sub>
</p>

<p align="center">
  <sub>&copy; 2026 Green Horizon Innovation LLC. All rights reserved.</sub>
</p>
