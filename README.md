# Passiv

Earn USDC from the behavioral data your wallet already creates.

Passiv is a protocol on Base that computes a **k-anonymized behavioral cohort profile** from an opted-in wallet's public onchain history and sells cohort access to automated buyers via x402 micropayments — paying the wallet owner 80% of query revenue in USDC. Off-chain compute, on-chain settlement and custody. No token in v1.

## Status
Early development. Product code lands here as it's built.

## How it works (target architecture)
1. **Indexer** reads a connected wallet's full Base history (Envio HyperSync + custom DeFi decoders) into Postgres.
2. **Fingerprint + privacy** computes a behavioral vector, then enforces k-anonymity (cohorts ≥ 50), feature bucketing, rare-combination suppression, and per-category opt-out.
3. **x402 oracle** (Fastify) answers filtered cohort queries; each query is gated by an x402 micropayment in USDC on Base.
4. **Earnings + settlement** splits 80/20 by cohort membership into an idempotent ledger reconciled to an on-chain `EarningsVault`.
5. **Withdrawal** is gasless via an ERC-4337 paymaster (above a minimum threshold).
6. **Dashboard** (Next.js) shows opt-in controls, an earnings ticker, an anonymized query feed, and withdraw/delete.

## Honest positioning
Profiles are **pseudonymous, not anonymous** — they are derived from public chain data and may be correlatable to a wallet by a third party who holds that data. Passiv mitigates this with k-anonymity cohorts and never serves single-profile responses, but makes no absolute anonymity guarantee.

## Stack
TypeScript monorepo (pnpm + Turborepo) · Fastify oracle · Next.js + wagmi/viem/OnchainKit dashboard · Solidity + Foundry contracts · x402 v2 + Coinbase CDP facilitator/paymaster · Postgres + Redis · Noir ZK (Phase 3).

## License
Core + contracts: Apache-2.0. SDKs: MIT.
