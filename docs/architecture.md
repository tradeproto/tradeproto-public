# TradeProto Technical Architecture

## System Layers

TradeProto is organized into five architectural layers, each responsible for a distinct domain of the platform's functionality.

```
  ┌─────────────────────────────────────────────────────────────────────┐
  │                        LAYER 5: AI / AGENTS                        │
  │         QM2ARL Multi-Agent Reinforcement Learning Framework         │
  │         ENGRAM Knowledge Graph · Gemini 2.5 Flash Extraction        │
  ├─────────────────────────────────────────────────────────────────────┤
  │                       LAYER 4: DATA                                 │
  │           Space and Time · Proof of SQL · Merkle Anchoring          │
  │           Clients · Documents · Seals · Attestations                │
  ├─────────────────────────────────────────────────────────────────────┤
  │                    LAYER 3: SETTLEMENT                              │
  │       Smart Contracts · TRNT Token · SBLC Escrow · B/L Transfer    │
  │       ERC-3643 Gated Transfers · Oracle Router Price Feeds          │
  ├─────────────────────────────────────────────────────────────────────┤
  │                    LAYER 2: COMPLIANCE                              │
  │         Circom ZK Circuits · KYC Verifier · SGS Inspector           │
  │         Maritime B/L Verifier · Biometric ID Verifier               │
  ├─────────────────────────────────────────────────────────────────────┤
  │                    LAYER 1: IDENTITY                                │
  │            Dragon Seal · Soulbound ERC-721 · SHA-256 Hash           │
  │            Biometric Composite · Holographic Seal · Signature       │
  └─────────────────────────────────────────────────────────────────────┘
```

---

## Layer 1: Identity (Dragon Seal)

The identity layer is the foundation of the entire system. Every participant in the TradeProto ecosystem is represented by a **Dragon Seal** -- a Soulbound ERC-721 NFT that is non-transferable and permanently bound to its owner.

### Components

- **Holographic Seal Generator** -- Creates a unique visual seal per identity
- **Digital Signature Capture** -- Cryptographic signature binding
- **Passport Biometric Scanner** -- Extracts biometric data for composite hash
- **SHA-256 Composite Hash** -- Combines biometric, seal, and signature into a single immutable hash
- **DragonSeal Contract** -- ERC-721 Soulbound NFT contract
- **DragonSealRegistry** -- On-chain registry mapping wallet addresses to seal token IDs

### Identity Data Flow

```
Passport Scan ──┐
                 ├──> SHA-256 Composite ──> DragonSeal.mint() ──> On-Chain Identity
Signature ───────┤
Holographic ─────┘
```

---

## Layer 2: Compliance (ZK Circuits)

Zero-knowledge circuits allow the platform to prove regulatory compliance without revealing the underlying confidential data.

### Compiled Circuits

| Circuit | Purpose | Inputs | Public Output |
|---|---|---|---|
| **KYC Verifier** | Proves identity documents are valid and match on-chain claims | Document hashes, identity claims | Boolean: valid/invalid |
| **SGS Inspector** | Proves product inspection results meet contractual specs | Inspection data, spec thresholds | Boolean: pass/fail |
| **Maritime B/L** | Proves Bill of Lading authenticity and shipment details | B/L document hash, vessel data | Boolean: authentic/forged |
| **Biometric ID** | Proves biometric data matches Dragon Seal composite hash | Biometric input, stored hash | Boolean: match/mismatch |

### ZK Proof Generation Flow

```
Private Inputs ──> Circom Circuit ──> Witness Generation ──> Groth16 Proof ──> On-Chain Verification
```

All circuits use the **Groth16** proving system for efficient on-chain verification with constant-size proofs.

---

## Layer 3: Settlement (Smart Contracts)

### Contract Overview

| Contract | Standard | Description |
|---|---|---|
| **TradeProto Token (TRNT)** | ERC-20 + ERC-3643 | Governance and utility token. 100M max supply. KYC/AML gated transfers. |
| **IdentityRegistry** | ERC-3643 | Maps wallet addresses to verified identity claims. Only trusted issuers can write claims. |
| **ModularCompliance** | ERC-3643 | Pluggable compliance modules that gate every token transfer (country restrictions, investor limits, lock periods). |
| **DragonSeal** | ERC-721 (Soulbound) | Non-transferable identity NFT. One per participant. Stores biometric composite hash. |
| **DragonSealRegistry** | Custom | Maps addresses to Dragon Seal token IDs. Provides lookup and verification functions. |
| **SBLCEscrow** | Custom | Holds tokenized SBLC collateral. Atomic release on B/L confirmation. |
| **OracleRouter** | Custom | Aggregates multi-source price feeds with staleness detection and deviation circuit breakers. |

### Token Transfer Flow (ERC-3643)

```
Transfer Request
      │
      v
IdentityRegistry.isVerified(from) ──> false ──> REVERT
      │ true
      v
IdentityRegistry.isVerified(to) ──> false ──> REVERT
      │ true
      v
ModularCompliance.canTransfer(from, to, amount) ──> false ──> REVERT
      │ true
      v
Transfer Executed
```

---

## Layer 4: Data (Space and Time)

All platform data is stored in **Space and Time**, a decentralized data warehouse that provides **Proof of SQL** -- cryptographic proof that query results have not been tampered with.

### Database Schema

#### `clients`

| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `wallet_address` | VARCHAR(42) | Ethereum wallet address |
| `dragon_seal_id` | INTEGER | Dragon Seal NFT token ID |
| `kyc_status` | ENUM | PENDING, VERIFIED, REJECTED |
| `company_name` | VARCHAR | Corporate entity name |
| `jurisdiction` | VARCHAR | Registered jurisdiction |
| `created_at` | TIMESTAMP | Registration timestamp |

#### `documents`

| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `client_id` | UUID | Foreign key to clients |
| `doc_type` | ENUM | KYC, CIS, SPA, FCO, LOI, BL, SGS |
| `doc_hash` | VARCHAR(66) | SHA-256 hash of document content |
| `ipfs_cid` | VARCHAR | IPFS content identifier |
| `status` | ENUM | UPLOADED, ATTESTED, EXPIRED |
| `created_at` | TIMESTAMP | Upload timestamp |

#### `seals`

| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `client_id` | UUID | Foreign key to clients |
| `token_id` | INTEGER | On-chain ERC-721 token ID |
| `biometric_hash` | VARCHAR(66) | SHA-256 biometric composite |
| `seal_hash` | VARCHAR(66) | SHA-256 holographic seal hash |
| `signature_hash` | VARCHAR(66) | SHA-256 digital signature hash |
| `mint_tx` | VARCHAR(66) | Ethereum transaction hash |
| `minted_at` | TIMESTAMP | Mint timestamp |

#### `attestations`

| Column | Type | Description |
|---|---|---|
| `id` | UUID | Primary key |
| `seal_id` | UUID | Foreign key to seals |
| `document_id` | UUID | Foreign key to documents |
| `attestation_hash` | VARCHAR(66) | SHA-256 attestation hash |
| `tx_hash` | VARCHAR(66) | Ethereum transaction hash |
| `attested_at` | TIMESTAMP | Attestation timestamp |

### Merkle Anchoring

Database state is periodically committed to Ethereum L1 as Merkle roots, providing an immutable audit trail:

```
Space and Time Tables ──> Merkle Tree ──> Root Hash ──> Ethereum L1 Transaction
```

---

## Layer 5: AI / Agents

### KYC Extraction Agent

The KYC extraction agent uses **Gemini 2.5 Flash** to automatically parse client-submitted documents and populate a standardized 12-section bank template.

#### 12-Section Bank Template

| Section | Content |
|---|---|
| 1. Entity Information | Company name, registration number, jurisdiction |
| 2. Beneficial Ownership | UBO details, ownership percentages |
| 3. Directors & Officers | Names, nationalities, DOBs |
| 4. Authorized Signatories | Signatory details with specimen signatures |
| 5. Business Description | Nature of business, products, markets |
| 6. Source of Funds | Revenue sources, banking relationships |
| 7. Source of Wealth | Historical wealth accumulation |
| 8. Tax Information | TIN, tax residency, FATCA/CRS status |
| 9. Sanctions Screening | OFAC, EU, UN sanctions check results |
| 10. PEP Screening | Politically Exposed Persons check |
| 11. Risk Assessment | Risk rating, risk factors, mitigants |
| 12. Supporting Documents | Passport copies, utility bills, bank references |

### QM2ARL / ENGRAM Agent Framework

The **Quantum Multi-Agent Multi-Modal Reinforcement Learning** (QM2ARL) framework and its associated **ENGRAM** knowledge graph power advanced reasoning across the platform:

- **Trade Intelligence** -- Pattern recognition across commodity markets
- **Geophysical Analysis** -- Subsurface anomaly detection via TELPAI
- **Compliance Monitoring** -- Real-time transaction screening
- **Document Intelligence** -- Cross-referencing trade documents for inconsistencies

---

## Oracle Architecture

### Multi-Source Price Feeds

The Oracle Router aggregates price data from multiple independent sources to ensure accuracy and resilience.

```
Source A (API) ──┐
Source B (API) ──┼──> OracleRouter ──> Median Price ──> Smart Contracts
Source C (API) ──┘
```

### Staleness Detection

Each price feed includes a `lastUpdated` timestamp. The Oracle Router rejects any price that exceeds the configured staleness threshold (default: 1 hour for commodity prices).

### Deviation Circuit Breakers

If the incoming price deviates from the stored price by more than a configured threshold (e.g., 5%), the circuit breaker activates:

1. **Pause** -- Trading is paused for the affected commodity
2. **Alert** -- Platform operators are notified
3. **Manual Override** -- Operators can confirm or reject the new price
4. **Resume** -- Trading resumes with the confirmed price

---

## Security Considerations

- All smart contracts are built on **OpenZeppelin** battle-tested implementations
- ZK circuits compiled with **Circom 2.x** and proven with **Groth16**
- Key management follows institutional custody standards
- All API endpoints require authenticated sessions
- Database queries are verified via **Proof of SQL** -- no trust assumptions

---

*For the complete trade lifecycle, see [trade-flow.md](trade-flow.md).*

*For Dragon Seal specification, see [dragon-seal.md](dragon-seal.md).*
