# Dragon Seal -- Soulbound Identity Verification

## Overview

Dragon Seal is a **Soulbound ERC-721 NFT** that serves as the cryptographic identity layer for the TradeProto ecosystem. Each Dragon Seal is:

- **Non-transferable** -- permanently bound to its owner's wallet
- **One per identity** -- a single seal per verified participant
- **Biometrically anchored** -- SHA-256 composite hash of biometric, seal, and signature data
- **Living** -- grows stronger with every document it attests

Traditional PDF stamps and rubber seals on trade documents are trivially copied and forged. In commodity trading, a single forged seal on a KYC, SPA, or FCO can enable millions in fraudulent transactions. Dragon Seal eliminates this attack vector entirely.

---

## How It Works

### Step 1: Identity Capture

The identity capture process generates four cryptographic artifacts:

1. **Holographic Seal** -- A unique visual seal generated per identity
2. **Digital Signature** -- Cryptographic signature capture
3. **Passport Biometric** -- Biometric data extraction from passport scan
4. **SHA-256 Composite Hash** -- All three artifacts combined into a single hash

### Step 2: Dragon Seal Mint

A Soulbound ERC-721 NFT is minted to the participant's wallet:

- Token is **non-transferable** (transfer functions are overridden to revert)
- Biometric composite hash is stored on-chain
- One token per wallet address enforced at the contract level

### Step 3: Document Attestation

Each document in the trade lifecycle is attested against the Dragon Seal:

| Document | Full Name | Purpose |
|---|---|---|
| **KYC** | Know Your Customer | Identity verification and regulatory compliance |
| **CIS** | Client Information Sheet | Corporate profile, banking details, authorized signatories |
| **SPA** | Sale & Purchase Agreement | Transaction terms, pricing, delivery schedules |
| **FCO** | Full Corporate Offer | Seller's formal commodity offer with specifications |
| **LOI** | Letter of Intent | Buyer's formal expression of interest |

For each attestation:
- The document is SHA-256 hashed
- The hash is written on-chain against the Dragon Seal token ID
- An immutable timestamp is recorded
- The Ethereum transaction hash serves as proof of attestation

### Step 4: Verification

Any counterparty can verify a Dragon Seal's authenticity by:

1. Looking up the seal by wallet address or token ID
2. Checking the attestation chain for the relevant document type
3. Verifying the document hash matches the on-chain record
4. Confirming the attestation timestamp and transaction hash

No trust required -- only on-chain verification.

---

## `.dragon` File Format Specification

The `.dragon` file is a self-contained verification package that bundles identity artifacts, attestation history, and on-chain proofs into a single exportable file.

### File Structure

```json
{
  "format": "dragon",
  "version": "1.0.0",
  "magic": "DRGN",
  "created": "2026-03-15T14:30:00Z",

  "identity": {
    "biometricHash": "sha256:a7f3b2e1c9d4f8a6b3e7d2c1f5a9b8e4d6c3a1f7b2e9d4c8a6f3b1e5d2c7a9",
    "sealHash":      "sha256:3e8f1a2b5c9d7e4f6a3b8c1d2e5f9a7b4c6d8e3f1a2b5c9d7e4f6a3b8c1d2e",
    "signatureHash": "sha256:7d4e2f1a8b3c6d9e5f2a7b4c1d8e3f6a9b2c5d8e1f4a7b3c6d9e2f5a8b1c4d",
    "compositeHash": "sha256:f1a2b3c4d5e6f7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d9e0f1"
  },

  "seal": {
    "tokenId": 42,
    "chain": "ethereum:1",
    "contract": "0x7B2c4f8E9a1D3b6C5e2A7f0B8d4E6c3A1F9b5D2",
    "mintTx": "0x4f2a8b1c3e7d9f6a5b2c4e8d1f3a7b9c6d2e5f8a1b4c7d3e9f6a2b5c8d1e4f",
    "mintBlock": 19847263,
    "mintTimestamp": "2026-01-20T09:15:00Z",
    "owner": "0x1234567890abcdef1234567890abcdef12345678"
  },

  "attestations": [
    {
      "type": "KYC",
      "documentHash": "sha256:b2e1c9d4f8a6b3e7d2c1f5a9b8e4d6c3a1f7b2e9d4c8a6f3b1e5d2c7a9f4b6",
      "tx": "0x4f2a8b1c3e7d9f6a5b2c4e8d1f3a7b9c6d2e5f8a1b4c7d3e9f6a2b5c8d1e4f",
      "block": 19847300,
      "timestamp": "2026-01-20T10:30:00Z",
      "issuer": "0xTrustedIssuerAddress"
    },
    {
      "type": "CIS",
      "documentHash": "sha256:c9d4f8a6b3e7d2c1f5a9b8e4d6c3a1f7b2e9d4c8a6f3b1e5d2c7a9f4b6e1d3",
      "tx": "0x8b1c3e7d9f6a5b2c4e8d1f3a7b9c6d2e5f8a1b4c7d3e9f6a2b5c8d1e4f2a7b",
      "block": 19847350,
      "timestamp": "2026-01-20T11:00:00Z",
      "issuer": "0xTrustedIssuerAddress"
    },
    {
      "type": "SPA",
      "documentHash": "sha256:e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1b2c3d4e5",
      "tx": "0xa3d7b2e1c9f4a8b6d3e5c1f7a9b2d4e8c6f3a1b5d9e2c7f4a8b1d6e3c5f9a2",
      "block": 19850100,
      "timestamp": "2026-01-22T14:00:00Z",
      "issuer": "0xTrustedIssuerAddress"
    },
    {
      "type": "FCO",
      "documentHash": "sha256:a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a5b6c7d8e9f0a1",
      "tx": "0xb5c8d1e4f2a7b3e9f6a2b5c8d1e4f2a7b3e9f6a2b5c8d1e4f2a7b3e9f6a2b5",
      "block": 19850200,
      "timestamp": "2026-01-22T15:30:00Z",
      "issuer": "0xTrustedIssuerAddress"
    }
  ],

  "verification": {
    "rpcEndpoint": "https://mainnet.infura.io/v3/...",
    "explorerUrl": "https://etherscan.io/token/0x7B2c...?a=42",
    "verifyInstructions": "Call DragonSealRegistry.verifySeal(owner, tokenId) to confirm on-chain."
  }
}
```

### Magic Bytes

All `.dragon` files begin with the ASCII bytes `DRGN` (0x4452474E) when stored in binary format.

### MIME Type

```
application/x-dragon+json
```

### File Extension

```
.dragon
```

---

## Browser SDK Usage

### Verifying a Dragon Seal

```javascript
import { DragonSealSDK } from '@tradeproto/dragon-seal-sdk';

const sdk = new DragonSealSDK({
  rpcUrl: 'https://mainnet.infura.io/v3/YOUR_KEY',
  contractAddress: '0x7B2c...DragonSeal',
  registryAddress: '0x9A1f...DragonSealRegistry'
});

// Verify a seal by wallet address
const result = await sdk.verifySeal('0x1234...owner');
console.log(result.isValid);       // true
console.log(result.tokenId);       // 42
console.log(result.attestations);  // [{type: 'KYC', ...}, ...]
```

### Loading a .dragon File

```javascript
// Parse a .dragon file
const dragonFile = await sdk.parseDragonFile(fileBuffer);

// Verify all attestations against on-chain records
const verification = await sdk.verifyDragonFile(dragonFile);
console.log(verification.allValid);              // true
console.log(verification.attestationResults);    // [{type: 'KYC', valid: true}, ...]
```

### Attesting a Document

```javascript
// Attest a document against a Dragon Seal (requires trusted issuer role)
const docHash = sdk.hashDocument(documentBuffer);  // SHA-256
const tx = await sdk.attestDocument({
  sealTokenId: 42,
  documentType: 'SPA',
  documentHash: docHash
});
console.log(tx.hash);  // 0x...
```

---

## Smart Contract Interface

### DragonSeal.sol (Soulbound ERC-721)

```solidity
interface IDragonSeal {
    /// @notice Mint a new Dragon Seal (only callable by authorized minters)
    function mintSeal(
        address to,
        bytes32 biometricHash,
        bytes32 sealHash,
        bytes32 signatureHash
    ) external returns (uint256 tokenId);

    /// @notice Attest a document against a Dragon Seal
    function attestDocument(
        uint256 tokenId,
        string calldata docType,
        bytes32 documentHash
    ) external;

    /// @notice Get all attestations for a seal
    function getAttestations(uint256 tokenId)
        external view returns (Attestation[] memory);

    /// @notice Verify a specific document attestation
    function verifyAttestation(
        uint256 tokenId,
        string calldata docType,
        bytes32 documentHash
    ) external view returns (bool);
}
```

### Transfer Override (Soulbound)

```solidity
function _update(address to, uint256 tokenId, address auth)
    internal override returns (address)
{
    address from = _ownerOf(tokenId);
    // Allow minting (from == address(0)) but block all transfers
    if (from != address(0)) {
        revert SoulboundTransferBlocked();
    }
    return super._update(to, tokenId, auth);
}
```

---

## Document Attestation Flow

```
  Document Created
        │
        v
  SHA-256 Hash Generated
        │
        v
  DragonSeal.attestDocument(tokenId, docType, hash)
        │
        v
  On-Chain Event: DocumentAttested(tokenId, docType, hash, timestamp)
        │
        v
  Space and Time: Attestation record written with Proof of SQL
        │
        v
  .dragon file updated with new attestation entry
```

---

## Security Properties

| Property | Guarantee |
|---|---|
| **Non-transferable** | Transfer functions revert for all non-mint operations |
| **One-per-identity** | Contract enforces single token per wallet address |
| **Tamper-proof** | Document hashes are immutable once attested on-chain |
| **Publicly verifiable** | Any address can call verification functions |
| **Timestamped** | Block timestamp provides ordering and proof of time |
| **Biometric binding** | SHA-256 composite hash links physical identity to on-chain token |

---

*See also: [architecture.md](architecture.md) | [trade-flow.md](trade-flow.md)*
