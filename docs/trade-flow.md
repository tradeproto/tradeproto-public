# Trade Flow -- Complete Trade Lifecycle

## Overview

This document describes the complete lifecycle of a commodity trade on the TradeProto platform, from initial buyer onboarding through final settlement and Dragon Seal attestation.

---

## Trade Lifecycle Stages

```
  01          02          03           04            05
  BUYER       KYC         SBLC         SBLC          PRODUCT
  ONBOARD     VERIFY      ISSUANCE     MONETIZE      SOURCING
    │           │           │            │              │
    v           v           v            v              v
  ┌─────┐   ┌─────┐   ┌─────┐     ┌─────┐       ┌─────┐
  │ REG │──>│ ZK  │──>│BANK │────>│WEALTH│──────>│MATCH│
  │     │   │PROOF│   │MT760│     │PROTO │       │     │
  └─────┘   └─────┘   └─────┘     └─────┘       └─────┘
                                                    │
    ┌───────────────────────────────────────────────┘
    │
    v
  06          07          08           09            10
  SGS         MARITIME    BILL OF      SETTLEMENT    DRAGON SEAL
  INSPECT     TRACKING    LADING                     ATTESTATION
    │           │           │            │              │
    v           v           v            v              v
  ┌─────┐   ┌─────┐   ┌─────┐     ┌─────┐       ┌─────┐
  │QUAL │──>│ AIS │──>│B/L  │────>│ATOMIC│──────>│SEAL │
  │CHECK│   │TRACK│   │TITLE│     │SETTL │       │ATTEST│
  └─────┘   └─────┘   └─────┘     └─────┘       └─────┘
```

---

## Stage 1: Buyer Onboarding

### Process

1. Buyer registers on the TradeProto platform
2. Ethereum wallet is connected or created
3. Company information submitted via Client Information Sheet (CIS)
4. Preliminary OFAC/sanctions screening performed
5. Account created in `PENDING` status

### Required Information

- Legal entity name and registration number
- Jurisdiction of incorporation
- Beneficial ownership structure (UBO > 25%)
- Authorized signatories with specimen signatures
- Banking details (correspondent bank, SWIFT BIC)
- Product interest (crude, refined, LNG, etc.)

### Dragon Seal

If the buyer does not have a Dragon Seal, the identity capture process begins:
- Holographic seal generation
- Digital signature capture
- Passport biometric scan
- SHA-256 composite hash computation
- Dragon Seal NFT minted (Soulbound ERC-721)

---

## Stage 2: KYC Verification

### Automated Extraction

The **KYC Extraction Agent** (powered by Gemini 2.5 Flash) processes submitted documents:

1. Documents uploaded (passport, utility bill, bank reference, articles of incorporation)
2. Gemini 2.5 Flash extracts data into the **12-section bank template**
3. Extracted data reviewed by compliance officer
4. Corrections applied if needed

### ZK Proof Generation

Once KYC data is verified:

1. KYC data hashed (SHA-256)
2. Circom KYC Verifier circuit generates zero-knowledge proof
3. Proof verified on-chain
4. Trusted issuer writes KYC claim to IdentityRegistry
5. Wallet status updated to `VERIFIED`

### OFAC Screening

- Full SDN list screening for all principals, UBOs, and authorized signatories
- Country risk assessment
- PEP (Politically Exposed Persons) screening
- Adverse media screening

### Dragon Seal Attestation

KYC documents attested against the buyer's Dragon Seal:

```
DragonSeal.attestDocument(tokenId, "KYC", sha256(kycDocumentBundle))
```

---

## Stage 3: SBLC Issuance

### Process

1. Buyer requests SBLC from their bank for the trade amount
2. Bank issues MT799 (pre-advice) via SWIFT
3. Bank issues MT760 (SBLC) via SWIFT
4. SBLC details bridged on-chain via Oracle Router
5. SBLC verified by platform compliance

### SBLC Requirements

| Parameter | Typical Value |
|---|---|
| **Amount** | 100-110% of trade value |
| **Tenor** | 30-90 days (matches trade cycle) |
| **Issuing Bank** | Top 50 world bank by assets |
| **Beneficiary** | Seller or escrow entity |
| **Governing Law** | UCP 600 / ISP98 |

### On-Chain Verification

```
OracleRouter.verifySBLC({
  swiftRef: "MT760-2026-001234",
  issuingBank: "HSBC",
  amount: 50000000,        // $50M USD
  tenor: 90,               // days
  beneficiary: "0xSeller..."
})
```

---

## Stage 4: SBLC Monetization (WealthProto)

If the buyer needs operational liquidity, the SBLC can be monetized via WealthProto:

1. SBLC token minted (ERC-3643 compliant)
2. Token deposited as collateral in WealthProto vault
3. Buyer borrows stablecoins against SBLC collateral (up to 80% LTV)
4. Borrowed funds used for operational expenses
5. SBLC collateral released upon trade completion and repayment

This stage is **optional** -- buyers with sufficient liquidity may skip it.

---

## Stage 5: Product Sourcing

### Seller Matching

1. Platform matches buyer LOI with seller FCO
2. Product specifications verified (API gravity, sulfur content, pour point, etc.)
3. Price negotiated (Platts benchmark +/- differential)
4. Sale & Purchase Agreement (SPA) drafted and executed

### Product Specifications (Example: EN590 Diesel)

| Parameter | Specification |
|---|---|
| **Density at 15C** | 820-845 kg/m3 |
| **Cetane Number** | Min 51 |
| **Sulfur** | Max 10 ppm (Euro V) |
| **Flash Point** | Min 55C |
| **Viscosity at 40C** | 2.0-4.5 cSt |
| **Cloud Point** | Country-specific |

### Dragon Seal Attestation

SPA attested against both buyer and seller Dragon Seals:

```
DragonSeal.attestDocument(buyerTokenId, "SPA", sha256(spaDocument))
DragonSeal.attestDocument(sellerTokenId, "SPA", sha256(spaDocument))
```

---

## Stage 6: SGS Inspection

### Load Port Inspection

1. **SGS** (Societe Generale de Surveillance) inspector assigned to load port
2. Product quality tested against SPA specifications
3. Quantity measured (shore tank gauging, vessel ullage)
4. Certificate of Quality (COQ) issued
5. Certificate of Quantity issued

### ZK Proof

SGS inspection data processed through the SGS Inspector circuit:

```
SGS data (private) ──> Circom SGS Circuit ──> ZK Proof ──> On-chain verification
                                                              │
Result: PASS/FAIL (public)                                    │
No raw inspection data revealed                               v
                                                        Oracle update
```

---

## Stage 7: Maritime Tracking

### AIS Vessel Tracking

Once the vessel departs the load port, real-time tracking begins:

1. AIS (Automatic Identification System) data streamed via oracle
2. Vessel position, speed, heading updated on-chain
3. ETA calculations for discharge port
4. Deviation alerts if vessel strays from planned route
5. Sanctions screening on vessel flag state and ports of call

### Tracked Parameters

| Parameter | Source | Update Frequency |
|---|---|---|
| **Position** | AIS transponder | Every 6 minutes |
| **Speed** | AIS / calculated | Every 6 minutes |
| **Heading** | AIS transponder | Every 6 minutes |
| **Draft** | AIS (if equipped) | At port departure |
| **ETA** | Calculated | Hourly |
| **Flag State** | Vessel registry | Static |
| **Sanctions Status** | OFAC/EU screening | At departure + daily |

---

## Stage 8: Bill of Lading

### B/L Issuance

1. Master issues **clean on-board Bill of Lading** at load port
2. B/L details entered into the platform
3. B/L document SHA-256 hashed
4. Maritime B/L ZK circuit verifies authenticity

### B/L ZK Verification

```
B/L data (private) ──> Circom Maritime B/L Circuit ──> ZK Proof ──> On-chain
                                                                      │
Result: AUTHENTIC/FORGED (public)                                     │
No raw B/L data revealed                                              v
                                                              Settlement trigger
```

### B/L as Title Document

The Bill of Lading serves as title to the goods. Upon verified B/L submission:

1. Title transfers from seller to buyer (on-chain)
2. Settlement process is triggered
3. SBLC release process begins

---

## Stage 9: Settlement

### Atomic Settlement

Settlement occurs atomically -- all conditions must be met simultaneously:

```
  SETTLEMENT CONDITIONS (all required)
  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  [x] SBLC verified and locked              (Stage 3)
  [x] SGS inspection PASSED                 (Stage 6)
  [x] Vessel arrived at discharge port      (Stage 7)
  [x] B/L verified as authentic             (Stage 8)
  [x] All counterparties KYC verified       (Stage 2)
  [x] OFAC screening clear                  (Continuous)
  ─────────────────────────────────────────
  => SETTLEMENT EXECUTED
```

### Settlement Process

1. Smart contract verifies all conditions are met
2. Payment released from escrow (SBLC or stablecoin)
3. Title token transferred to buyer
4. SBLC collateral released (if monetized via WealthProto)
5. Platform fees collected
6. Settlement event emitted on-chain

### Payment Methods

| Method | Speed | Currency | Notes |
|---|---|---|---|
| **SWIFT MT103** | T+1 to T+3 | USD, EUR, GBP | Traditional bank wire |
| **Stablecoin** | T+0 (instant) | USDC, USDT | On-chain settlement |
| **SBLC Draw** | T+1 to T+5 | Per SBLC terms | Bank-to-bank |

---

## Stage 10: Dragon Seal Attestation

### Final Attestation

Upon successful settlement, the complete transaction chain is attested to both buyer and seller Dragon Seals:

```
Documents attested:
  - KYC (buyer + seller)           ──> attested at Stage 2
  - CIS (buyer + seller)           ──> attested at Stage 1
  - LOI (buyer)                    ──> attested at Stage 5
  - FCO (seller)                   ──> attested at Stage 5
  - SPA (buyer + seller)           ──> attested at Stage 5
  - SGS Report                     ──> attested at Stage 6
  - Bill of Lading                 ──> attested at Stage 8
  - Settlement Confirmation        ──> attested at Stage 9

Final attestation:
  DragonSeal.attestDocument(tokenId, "SETTLEMENT", sha256(settlementRecord))
```

### .dragon File Update

Both buyer and seller `.dragon` files are updated with the complete attestation chain from this trade, creating a permanent, verifiable record of the transaction.

---

## Timeline (Typical)

| Stage | Duration | Cumulative |
|---|---|---|
| Buyer Onboarding | 1-2 days | Day 1-2 |
| KYC Verification | 2-5 days | Day 3-7 |
| SBLC Issuance | 3-7 days | Day 6-14 |
| SBLC Monetization | 1 day (if needed) | Day 7-15 |
| Product Sourcing | 3-10 days | Day 10-25 |
| SGS Inspection | 1-2 days | Day 11-27 |
| Maritime Transit | 7-30 days | Day 18-57 |
| B/L Processing | 1 day | Day 19-58 |
| Settlement | 1-3 days | Day 20-61 |
| Dragon Seal Attestation | Immediate | Day 20-61 |

**Total cycle: 20-61 days** (varies by route and product)

---

*See also: [architecture.md](architecture.md) | [dragon-seal.md](dragon-seal.md) | [ofac-compliance.md](ofac-compliance.md) | [wealthproto.md](wealthproto.md)*
