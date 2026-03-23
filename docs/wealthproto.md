# WealthProto -- DeFi Liquidity & SBLC Monetization

## Overview

WealthProto is the **Liquidity Layer for Global Energy Infrastructure**. It transforms Standby Letters of Credit (SBLCs) and infrastructure bonds into programmable collateral, enabling energy leaders to access global capital markets for project financing with full transparency and verified compliance.

---

## SBLC Monetization Flow

### What is an SBLC?

A **Standby Letter of Credit** is a guarantee issued by a bank on behalf of a client, ensuring payment to a beneficiary if the client fails to fulfill contractual obligations. In commodity trading, SBLCs serve as financial guarantees for large-volume transactions.

### Traditional SBLC Limitations

- **Illiquid** -- Locked capital sitting idle in banking instruments
- **Opaque** -- Paper-based, faxed between correspondent banks
- **Slow** -- Days to weeks for verification and settlement
- **Single-use** -- Cannot be fractionalized or leveraged

### WealthProto SBLC Monetization

WealthProto tokenizes SBLCs into on-chain collateral positions that can be:

1. **Verified** -- SWIFT MT760/MT799 data bridged on-chain via oracle
2. **Tokenized** -- Represented as ERC-3643 compliant security tokens
3. **Fractionalized** -- Split into smaller tradeable positions
4. **Leveraged** -- Used as collateral in DeFi lending protocols
5. **Settled** -- Atomically released on trade completion

### Monetization Flow

```
  BANK                        WEALTHPROTO                      DeFi
  ━━━━                        ━━━━━━━━━━━                      ━━━━

  ┌──────────────┐     ┌───────────────────┐     ┌──────────────────┐
  │ Issuing Bank │     │ SBLC Tokenization │     │ Liquidity Pool   │
  │              │────>│ Engine            │────>│                  │
  │ MT760 SBLC   │     │                   │     │ SBLC-backed      │
  │ Issuance     │     │ - Oracle verify   │     │ yield positions  │
  │              │     │ - ZK compliance   │     │                  │
  └──────────────┘     │ - Token mint      │     │ - Lend/Borrow    │
                       │ - Collateral lock │     │ - Yield farming  │
  ┌──────────────┐     │                   │     │ - Flash loans    │
  │ Advising     │────>│                   │     │                  │
  │ Bank         │     └───────────────────┘     └──────────────────┘
  │              │              │
  │ MT799 Pre-   │              v
  │ Advice       │     ┌───────────────────┐
  └──────────────┘     │ Settlement Engine │
                       │                   │
                       │ - B/L confirmed   │
                       │ - SGS verified    │
                       │ - Atomic release  │
                       │ - Collateral free │
                       └───────────────────┘
```

---

## Tokenized Collateral Positions

### SBLC Token Properties

| Property | Value |
|---|---|
| **Standard** | ERC-3643 (regulated security token) |
| **Compliance** | KYC/AML verified holders only |
| **Backing** | 1:1 backed by verified bank SBLC |
| **Oracle** | SWIFT message verification via oracle bridge |
| **Fractionalization** | Minimum 0.01% of face value |
| **Maturity** | Matches underlying SBLC tenor |

### Collateral Lifecycle

```
  SBLC Issued (MT760)
        │
        v
  Oracle Verification ──> SWIFT data bridged on-chain
        │
        v
  ZK Compliance Check ──> Issuing bank, amount, tenor verified
        │
        v
  Token Minted ──> ERC-3643 SBLC token created
        │
        v
  Collateral Locked ──> Token deposited in escrow contract
        │
        ├──> Yield Position ──> Earn yield while locked
        │
        v
  Trade Completed ──> B/L + SGS confirmed
        │
        v
  Collateral Released ──> Token burned, SBLC returned to bank
```

---

## DeFi Liquidity Layer

### Lending & Borrowing

SBLC-backed tokens can be used as collateral in DeFi lending protocols:

- **Borrow against SBLC** -- Use tokenized SBLC as collateral to borrow stablecoins for operational capital
- **Earn yield on idle SBLCs** -- Deposit SBLC tokens into lending pools to earn interest
- **Flash loans** -- Institutional-grade flash loans backed by verified bank instruments

### Yield Sources

| Source | Mechanism | Expected APY |
|---|---|---|
| **Lending** | Supply SBLC tokens to lending pool | 3-6% |
| **Liquidity Provision** | SBLC/USDC pool on AMM | 5-12% |
| **Trade Finance Yield** | Settlement fees from commodity trades | 2-4% |
| **Staking** | TRNT governance token staking rewards | Variable |

### Risk Management

- **Over-collateralization** -- Minimum 150% collateral ratio for borrowing
- **Liquidation engine** -- Automatic liquidation if collateral ratio drops below 120%
- **Oracle price feeds** -- Real-time SBLC valuation via multi-source oracle
- **Insurance fund** -- Platform reserve for black swan events
- **Compliance gating** -- Only ERC-3643 verified participants can interact

---

## Infrastructure Bond Tokenization

Beyond SBLCs, WealthProto enables tokenization of infrastructure bonds for energy project financing.

### Supported Bond Types

| Bond Type | Use Case | Typical Tenor |
|---|---|---|
| **Project Finance Bonds** | Oil & gas exploration, pipeline construction | 5-15 years |
| **Green Bonds** | Renewable energy projects, carbon capture | 5-10 years |
| **Revenue Bonds** | Backed by project revenue streams | 7-20 years |
| **Convertible Notes** | Early-stage energy infrastructure | 2-5 years |

### Tokenization Benefits

- **Fractional ownership** -- Access to institutional-grade bonds at lower minimums
- **24/7 trading** -- Secondary market liquidity on-chain
- **Transparent cash flows** -- Coupon payments automated via smart contracts
- **Global access** -- KYC-verified investors worldwide can participate
- **Instant settlement** -- T+0 instead of T+2

### Bond Token Structure

```
  ┌───────────────────────────────────────┐
  │           INFRASTRUCTURE BOND          │
  │                                        │
  │   Face Value:    $10,000,000           │
  │   Coupon:        5.5% semi-annual      │
  │   Tenor:         10 years              │
  │   Rating:        BBB+                  │
  │                                        │
  │   ┌────────────────────────────────┐   │
  │   │      TOKENIZED TRANCHES       │   │
  │   │                                │   │
  │   │  Tranche A: 1,000 tokens       │   │
  │   │  @ $10,000 each                │   │
  │   │  Senior, first-loss protection │   │
  │   │                                │   │
  │   │  Tranche B: 500 tokens         │   │
  │   │  @ $10,000 each                │   │
  │   │  Mezzanine, higher yield       │   │
  │   └────────────────────────────────┘   │
  └───────────────────────────────────────┘
```

---

## Integration with TradeProto

WealthProto is fully integrated with the TradeProto commodity trading platform:

1. **Buyer requests SBLC** -- Via TradeProto trade initiation
2. **Bank issues SBLC** -- MT760 via SWIFT
3. **WealthProto tokenizes** -- SBLC becomes on-chain collateral
4. **Trade executes** -- Commodity sourced, shipped, inspected
5. **Settlement** -- B/L confirmed, SBLC atomically released
6. **Dragon Seal attestation** -- Entire transaction chain attested

This creates a seamless flow from banking instruments to DeFi liquidity and back, all within a compliance-verified framework.

---

*See also: [trade-flow.md](trade-flow.md) | [architecture.md](architecture.md) | [ofac-compliance.md](ofac-compliance.md)*
