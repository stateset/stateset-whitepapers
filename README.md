# StateSet Whitepapers

Technical whitepapers for the StateSet commerce infrastructure stack — a vertically integrated system enabling autonomous AI agents to execute, verify, and settle commerce operations without human intervention.

## The StateSet Trilogy

```
Layer 3 - Application      StateSet iCommerce
                            AI agents, MCP tools, A2A protocol
                                      |
                            VES events (signed, encrypted)
                                      v
Layer 2 - Ordering          StateSet Sequencer
                            Deterministic ordering, Merkle trees
                            STARK compliance proofs
                                      |
                            Merkle commitments (batched)
                                      v
Layer 1 - Settlement        SET Chain L2
                            SetRegistry, SetPaymaster, ssUSD
                            On-chain anchoring, gas abstraction
                                      |
                            State roots, fault proofs
                                      v
                            [  Ethereum (L1 DA)  ]
```

Every layer is independently verifiable. No layer trusts the one above it.

## Translations

All three whitepapers are available in:

| Language | Directory | Format |
|----------|-----------|--------|
| English  | [`en/`](./en/) | PDF |
| 中文 (Chinese) | [`cn/`](./cn/) | DOCX |
| 日本語 (Japanese) | [`jp/`](./jp/) | DOCX |

## Whitepapers

### 1. [StateSet iCommerce](./en/1_stateset_icommerce_whitepaper.pdf)
**An Embedded Commerce Engine for Autonomous AI Agents** | v0.7.15 | March 2026

StateSet iCommerce is an embedded, zero-dependency commerce engine designed for autonomous AI agents. Built on a Rust core with language bindings for 10 platforms, it provides a complete commerce and ERP surface area — orders, inventory, payments, returns, subscriptions, manufacturing, and more — as deterministic, locally executable operations that AI agents can safely invoke.

Key contributions:
- **365+ MCP tools** across 39 domain-specific modules — the largest known domain-specific MCP server
- **Agent-to-Agent (A2A) Commerce Protocol** for autonomous economic transactions between AI agents
- **x402 Payment Protocol** for cryptographically verifiable payment intents with Ed25519 signatures
- **Verifiable Encrypted Signatures (VES) v1.0** for tamper-proof event synchronization
- **Declarative policy engine** with deny-override semantics and explainable denials
- **21 Rust crates** with zero I/O side effects in the core logic, 10 language bindings via stable C ABI
- **18 specialized agent configurations** (customer-service, checkout, inventory, returns, analytics, etc.)
- **10,000+ automated tests** across Rust core, CLI, admin dashboard, and cross-language crypto vectors

Design principles: local-first execution (SQLite default), deterministic operations, type safety through newtypes, explicit state machines, and preview-before-execute.

---

### 2. [StateSet Sequencer](./en/2_stateset_sequencer_whitepaper.pdf)
**A Verifiable Event Ordering Service for Autonomous Commerce** | v0.2.5 | March 2026

The StateSet Sequencer is a deterministic event ordering service that provides the cryptographic backbone for distributed commerce systems. It implements the VES v1.0 protocol — a specification for producing gap-free, Ed25519-signed, Merkle-committed event streams that can be independently audited without trusting the sequencer itself.

Key contributions:
- **VES v1.0 protocol** with 10 domain-separated hash prefixes preventing cross-protocol collision attacks
- **Gap-free ordering guarantee** via PostgreSQL `SELECT FOR UPDATE` for linearizable sequencing
- **Commitment chaining** preventing history forking — each batch links to the previous via Merkle roots
- **End-to-end encryption (VES-ENC-1)** using AES-256-GCM + HPKE, allowing the sequencer to order encrypted events without seeing plaintext
- **Zero-knowledge compliance proofs** via STARK integration (Winterfell) for proving regulatory compliance without revealing transaction data
- **Agent key management** with registration, rotation, and revocation via `(tenant_id, agent_id, key_id)`
- **Two-tier finality**: soft finality (milliseconds, sequencer receipt) and hard finality (minutes, on-chain anchor)

Architecture: Rust service on Axum + PostgreSQL, with STARK prover for compliance and SET Chain L2 for anchoring.

---

### 3. [SET Chain](./en/3_set_chain_whitepaper.pdf)
**A Commerce-Optimized Layer 2 for Verifiable Agent Transactions** | Chain ID 84532001 | OP Stack v1.8.0 | March 2026

SET Chain is an Ethereum Layer 2 network purpose-built for commerce. Built on the OP Stack, it inherits Ethereum's security guarantees while providing 2-second block times, sub-cent transaction fees, and native infrastructure for the three primitives that autonomous commerce requires: verifiable event commitments (SetRegistry), gas-abstracted merchant transactions (SetPaymaster), and a yield-bearing stablecoin (ssUSD) backed by U.S. Treasury Bills.

Key contributions:
- **SetRegistry** — on-chain Merkle commitment storage enabling any third party to verify commerce events via `commitBatch()`, `verifyInclusion()`, and state chain continuity
- **SetPaymaster** — ERC-4337 gas sponsorship with tiered controls (Starter/Growth/Enterprise), so merchants and agents never manage gas tokens
- **ssUSD** — a rebasing stablecoin backed 1:1 by T-Bills yielding ~5% APY, with a non-rebasing ERC-4626 wrapper (wssUSD) for DeFi compatibility
- **Anchor Service** — Rust daemon bridging off-chain sequencer commitments to on-chain SetRegistry every 60 seconds with circuit breaker resilience
- **MEV protection** via phased strategy: private sequencer (current) through threshold encrypted mempool to forced L1 inclusion
- **Enshrined primitives** — SetRegistry, SetPaymaster, and ssUSD are network-level, not competing for block space

Chain parameters: 2s block time, 30M gas/block, EIP-4844 blob DA (~$0.001-0.01/batch), Ethereum Sepolia L1 settlement.

## Key Protocols

| Protocol | Purpose | Cryptography |
|----------|---------|-------------|
| A2A Commerce | Agent-to-agent transactions (payments, quotes, escrow, disputes, subscriptions) | Ed25519 wallets, ERC-8004 identity |
| x402 Payment | Off-chain payment intents with batch on-chain settlement | Ed25519 signatures, Merkle commitments |
| VES v1.0 | Tamper-proof event synchronization | RFC 8785 JCS, SHA-256, Ed25519, AES-256-GCM, Merkle trees |

## Supported Settlement Networks

| Network | Asset | Settlement Model |
|---------|-------|-----------------|
| SET Chain L2 | ssUSD (yield-bearing) | Native, fast finality, sub-cent gas |
| Solana | USDC | SPL token transfer via relayer |
| Base | USDC | ERC-4337 paymaster |
| Ethereum | USDC | ERC-4337 paymaster |
| Arbitrum | USDC | ERC-4337 paymaster |
| Bitcoin | BTC | UTXO-based |
| Zcash | ZEC | Privacy-preserving |

## License

StateSet iCommerce: MIT OR Apache-2.0

## Copyright

2026 StateSet Inc.
