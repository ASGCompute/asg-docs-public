---
title: "Payment Contract"
description: "Canonical 402 payment contract for ASG Agent Cloud"
---

# Payment Contract Reference

Every paid tool call on ASG follows the **402 Payment Required** flow. This document describes the canonical response shape that agents must parse to build Solana payment instructions.

## 402 Response Shape

When an agent calls a tool without payment, the gateway returns:

```json
{
  "code": "PAYMENT_REQUIRED",
  "message": "Payment required to execute tool",
  "request_id": "req_abc123",
  "quote": {
    "quote_id": "qt_xyz789",
    "tool": "inference_chat",
    "price_usdc_microusd": 2400,
    "price_display": "$0.0024",
    "ttl_seconds": 300,
    "expires_at": "2026-02-08T10:05:00Z"
  },
  "payment_instructions": {
    "network": "solana-mainnet",
    "asset": "USDC",
    "pay_to": "ATA_ADDRESS_HERE",
    "destination": "ATA_ADDRESS_HERE",
    "usdc_mint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    "memo_format": "asg:{quote_id}",
    "methods": ["transfer", "solana_pay"],
    "x402_header_format": "base64url(JSON({tx_signature, quote_id}))",
    "gasless_supported": true,
    "payment_methods": ["wallet", "gasless"]
  },
  "step_id": "step_001",
  "retry": {
    "method": "POST",
    "headers": { "X-Payment": "<base64url-encoded payment proof>" }
  }
}
```

## Field Reference

| Field | Type | Description |
|:------|:-----|:------------|
| `network` | `string` | Always `solana-mainnet` |
| `asset` | `string` | Always `USDC` |
| `pay_to` | `string` | Treasury USDC token account (ATA) |
| `destination` | `string` | Same as `pay_to` (alias) |
| `usdc_mint` | `string` | USDC SPL token mint address |
| `memo_format` | `string` | Required memo: `asg:{quote_id}` |
| `methods` | `string[]` | Supported: `transfer`, `solana_pay` |
| `gasless_supported` | `boolean` | Whether gasless relay is available |
| `payment_methods` | `string[]` | `wallet` (direct) or `gasless` (relay) |

## Payment Flow

```
Agent                        ASG Gateway                    Solana
  │                              │                            │
  │── POST /mcp (tool call) ────►│                            │
  │◄── 402 + quote + pay_to ─────│                            │
  │                              │                            │
  │── Build USDC transfer ──────────────────────────────────►│
  │   (to pay_to, memo: asg:{quote_id})                      │
  │◄── tx_signature ────────────────────────────────────────│
  │                              │                            │
  │── POST /mcp (same call) ────►│                            │
  │   X-Payment: base64({        │                            │
  │     tx_signature, quote_id   │                            │
  │   })                         │── Verify on-chain ────────►│
  │                              │◄── Confirmed ──────────────│
  │◄── 200 + result + receipt ───│                            │
```

## X-Payment Header

The retry request must include `X-Payment` header with base64url-encoded JSON:

```json
{
  "tx_signature": "5Uj3...",
  "quote_id": "qt_xyz789"
}
```

## Important Notes

- **Network**: Only `solana-mainnet` is supported. Devnet/testnet payments are rejected.
- **Gasless**: ASG covers Solana transaction fees. Agents only need USDC — no SOL required.
- **TTL**: Quotes expire after `ttl_seconds` (default 300s / 5 min). Expired quotes return `QUOTE_EXPIRED`.
- **Idempotency**: Re-submitting the same `X-Payment` header returns the cached result without re-charging.
- **Base URL**: `https://agent.asgcompute.com`
