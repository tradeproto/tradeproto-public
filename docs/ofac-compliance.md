# OFAC Compliance Framework

## Overview

TradeProto implements a comprehensive OFAC (Office of Foreign Assets Control) compliance framework designed for institutional commodity trade finance. This framework ensures all transactions comply with U.S. sanctions regulations while enabling legitimate energy commodity trade through proper licensing and documentation channels.

---

## License Types

### General Licenses

General Licenses authorize certain categories of transactions without requiring specific application to OFAC. These are published in the Code of Federal Regulations and Federal Register notices.

**Relevant General Licenses for Energy Trade:**

- **GL 8** -- Transactions related to the exportation or reexportation of certain services
- **GL 13** -- Transactions related to the provision of certain financial services
- **GL 40/41** -- Authorizations related to certain transactions involving PdVSA (where applicable)

### Specific Licenses

Specific Licenses are granted on a case-by-case basis following a formal application to OFAC. Required when:

- Transaction does not fall under any General License
- Counterparty is on the SDN (Specially Designated Nationals) list
- Transaction involves a sanctioned jurisdiction without General License coverage
- Complex multi-jurisdictional structures require explicit authorization

### License Application Process

```
Compliance Review ──> OFAC Application (Form SF-1032) ──> OFAC Review (90-180 days)
      │                                                           │
      v                                                           v
Risk Assessment                                         License Granted / Denied
      │                                                           │
      v                                                           v
Documentation Package                                   Conditions & Reporting
```

---

## Transaction Flow: Venezuelan Crude & Refined Products

### Regulatory Context

Transactions involving Venezuelan-origin petroleum products require careful navigation of the sanctions framework. The following outlines the compliant transaction structure.

### Flow Architecture

```
  ORIGIN                    INTERMEDIARY                  DESTINATION
  ━━━━━━                    ━━━━━━━━━━━━                  ━━━━━━━━━━━

  Venezuela         Non-sanctioned                    End Buyer
  (Production)      Trading Entity                    (Verified KYC)
       │                  │                                │
       v                  v                                v
  ┌──────────┐     ┌─────────────┐                  ┌──────────────┐
  │ Crude /  │────>│ CIF Hub     │─────────────────>│ Delivery     │
  │ Refined  │     │ (Rotterdam  │                  │ Port         │
  │ Products │     │  or Houston)│                  │              │
  └──────────┘     └─────────────┘                  └──────────────┘
                         │
                         v
                   ┌─────────────┐
                   │ SGS/Saybolt │
                   │ Inspection  │
                   │ at Load Port│
                   └─────────────┘
```

### Key Requirements

1. **No U.S. Person Involvement** -- Unless authorized by specific license, no U.S. persons (including U.S. financial institutions) may be involved in the transaction
2. **Non-SDN Counterparties** -- All counterparties must be screened against the SDN list
3. **Licensed Entity Structure** -- Trading entities must hold appropriate OFAC licenses if engaging with sanctioned origins
4. **Documentation Chain** -- Complete paper trail from origin to delivery

---

## CIF Structure

### Rotterdam Hub

Rotterdam serves as the primary European hub for CIF-structured commodity trades.

| Element | Detail |
|---|---|
| **Incoterm** | CIF Rotterdam |
| **Inspection** | SGS Netherlands B.V. at load port and discharge port |
| **Insurance** | Marine cargo insurance, minimum 110% CIF value |
| **Freight** | Vessel charter or slot booking, clean B/L required |
| **Banking** | European correspondent banking (non-U.S. corridor) |
| **Storage** | Rotterdam tank terminal (Vopak, Royal Vopak, Koole) |

### Houston Hub

Houston serves as the primary U.S. Gulf hub (requires specific license for sanctioned-origin products).

| Element | Detail |
|---|---|
| **Incoterm** | CIF Houston |
| **Inspection** | SGS North America at load port and discharge port |
| **Insurance** | Marine cargo insurance, minimum 110% CIF value |
| **Freight** | Vessel charter, clean on-board B/L required |
| **Banking** | U.S. banking requires specific OFAC license |
| **Storage** | Houston Ship Channel terminal facilities |

### CIF Price Composition

```
CIF Price = FOB Price + Freight + Insurance + Inspection Fees

  FOB (Free on Board)     $XX.XX /bbl    Product cost at load port
  + Freight               $X.XX  /bbl    Maritime transport
  + Insurance             $0.XX  /bbl    Marine cargo (110% CIF)
  + Inspection            $0.XX  /bbl    SGS load + discharge
  ─────────────────────────────────────
  = CIF Price             $XX.XX /bbl    Delivered cost
```

---

## Required Documentation

Every compliant transaction requires the following documentation chain. All documents are Dragon Seal attested for immutable verification.

### Pre-Transaction Documents

| Document | Abbreviation | Purpose |
|---|---|---|
| Know Your Customer | KYC | Identity verification for all counterparties |
| Client Information Sheet | CIS | Corporate profile, banking details, authorized signatories |
| Letter of Intent | LOI | Buyer's formal expression of interest |
| Full Corporate Offer | FCO | Seller's formal commodity offer |
| Sale & Purchase Agreement | SPA | Binding transaction terms |
| OFAC License (if required) | -- | Specific license from OFAC |

### Transaction Documents

| Document | Abbreviation | Purpose |
|---|---|---|
| Standby Letter of Credit | SBLC | Financial guarantee instrument (MT760) |
| Proof of Product | POP | Evidence of product availability |
| SGS Inspection Report | SGS | Product quality and quantity verification |
| Bill of Lading | B/L | Maritime shipping document, title to goods |
| Certificate of Origin | COO | Product origin attestation |
| Certificate of Quality | COQ | Product specifications verification |

### Post-Transaction Documents

| Document | Abbreviation | Purpose |
|---|---|---|
| Discharge Report | -- | Quantity received at destination |
| Final Invoice | -- | Settlement invoice with adjustments |
| Quality Certificate (Discharge) | -- | Specs verification at discharge port |
| Payment Confirmation | -- | SWIFT MT103 confirmation |

---

## Banking Considerations

### Non-U.S. Banking Corridors

For transactions involving sanctioned-origin products without specific OFAC license, banking must flow through non-U.S. financial institutions to avoid U.S. nexus.

**Compliant Banking Corridors:**

| Corridor | Banks | Notes |
|---|---|---|
| **European** | Deutsche Bank, BNP Paribas, Credit Suisse, ING | EUR-denominated transactions preferred |
| **Middle East** | Emirates NBD, ADCB, Mashreq | Dubai hub for re-export transactions |
| **Asian** | DBS, OCBC, UOB (Singapore) | SGD or USD (non-U.S. correspondent) |
| **UK** | Standard Chartered, Barclays | GBP or EUR denominated |

### SWIFT Message Types

| Message | Purpose |
|---|---|
| **MT199** | Free-format message for bank-to-bank communication |
| **MT700** | Issue of a Documentary Credit |
| **MT760** | Standby Letter of Credit issuance |
| **MT799** | Free-format bank-to-bank (pre-advice for SBLC) |
| **MT103** | Single Customer Credit Transfer (payment) |

### U.S. Nexus Triggers

The following create U.S. jurisdiction nexus and require specific OFAC licensing:

- Transaction denominated in USD and clearing through U.S. correspondent bank
- Any U.S. person (individual or entity) involved as principal or agent
- U.S.-origin goods, technology, or services involved
- Transaction processed through U.S. financial system (CHIPS, Fedwire)

---

## Sanctions Screening Integration

### Automated Screening

TradeProto integrates automated sanctions screening at multiple checkpoints:

```
Entity Onboarding ──> SDN/OFAC Screen ──> Dragon Seal Mint (if clear)
         │
Transaction Initiation ──> Re-screen all counterparties ──> Proceed (if clear)
         │
Settlement ──> Final screen ──> Release funds (if clear)
```

### Screening Databases

- **OFAC SDN List** -- Specially Designated Nationals and Blocked Persons
- **OFAC Consolidated Sanctions List** -- All OFAC sanctions programs
- **EU Consolidated List** -- European Union sanctions
- **UN Security Council Sanctions** -- United Nations sanctions
- **UK HMT Sanctions** -- UK financial sanctions targets

### Match Handling

| Match Type | Action |
|---|---|
| **Exact Match** | Block transaction, file SAR, notify compliance officer |
| **Fuzzy Match (>85%)** | Hold transaction, manual review required |
| **Partial Match (<85%)** | Flag for review, transaction may proceed with approval |
| **No Match** | Transaction proceeds normally |

---

## Compliance Reporting

### Record Retention

All transaction records, screening results, and compliance decisions must be retained for a minimum of **5 years** from the date of the transaction, as required by OFAC regulations (31 CFR Part 501).

### Voluntary Self-Disclosure

If a potential violation is identified, TradeProto's compliance framework supports Voluntary Self-Disclosure (VSD) to OFAC, which can result in significantly reduced penalties.

---

*See also: [trade-flow.md](trade-flow.md) | [architecture.md](architecture.md)*
