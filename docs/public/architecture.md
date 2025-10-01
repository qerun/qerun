# Qerun Architecture Overview

## 1. Executive Summary
Qerun is a DAO-governed crypto ecosystem designed to give members a transparent, low-friction way to hold and grow value. The MVP focuses on four core capabilities: minting and tracking the fixed-supply QER token, managing treasury assets, enabling controlled swaps, and coordinating governance actions through a community-owned portal.

Key objectives for the MVP architecture:
- Preserve capital safety through conservative smart-contract design and multi-party controls.
- Keep components modular so future governance, social, and financial utilities can plug in without rewrites.
- Ensure every user-facing action is explainable and traceable (alerts, timelocks, immutable logs).
- Support a progressive decentralisation path: start with steward-controlled operations, then hand off powers to the DAO as tooling matures.

## 2. Context & Requirements
### Business context
- Community-first protocol where membership is represented by QER and governance NFTs.
- Initial audience: early adopters providing liquidity and helping define treasury strategy.
- Distribution and engagement rely on credibility; architecture must surface provable state at all times.

### Functional requirements
- Fixed-supply ERC-20 token (`QER`) with DAO-controlled mint permissions and null-address protections.
- Registry of protocol configuration (addresses, limits, feature flags) that other contracts can read without redeploys.
- Swap mechanism allowing participants to acquire/redeem QER with the chosen base asset (WETH or a stablecoin).
- Portfolio ledger that records the assets backing QER and exposes summary metrics to the dashboard.
- Governance hooks (timelock, multi-sig, voting adapters) to gate sensitive changes.
- Public-facing portal that retrieves on-chain data, shows alerts, and guides users through flows.

### Non-functional requirements
- Deterministic deployments with reproducible builds (Foundry/Hardhat, scripted migrations).
- Minimal external trust assumptions; every oracle or off-chain dependency must be documented.
- Gas efficiency in steady-state operations, but favour clarity and auditability over micro-optimisation.
- Uptime target for public portal ≥ 99% using static hosting + redundancy (IPFS pinning + fallback site).
- Security-first lifecycle: reviews, test coverage, and staged rollouts before DAO activation.

## 3. System Overview
The ecosystem is composed of three layers.

1. **Smart-contract layer (on-chain)**
   - `Qerun.sol` (ERC-20 token, supply control, security guards).
   - `StateManager.sol` (global configuration registry).
   - `Swap/DEX` contract (AMM or bonding-curve module for QER ↔ base asset).
   - `PortfolioManager.sol` (backing assets ledger, reserve accounting).
   - `GovernanceTimelock` + optional `MultiSig` (execution gating, emergency powers).

2. **Interface & integration layer (off-chain)**
   - Qerun Portal (Next.js static site served via IPFS/ENS) for token info, swap UI, governance updates.
   - Wallet tracker / alerting microservice (optional in MVP) that listens to contract events and pushes notifications.
   - Admin tooling (CLI scripts, ops dashboards) used by stewards and, eventually, elected DAO roles.

3. **Support services**
   - Data indexing (The Graph/Substreams or lightweight event listeners) to populate dashboards.
   - Storage and delivery (IPFS, optional Arweave mirror, CDN pinning.
   - Observability stack (logs from indexing services, alerting for contract anomalies).

## 4. Smart Contract Architecture
### 4.1 StateManager.sol
- Namespaced key/value store (`module.key`) for addresses, protocol parameters, feature flags.
- Only governance or designated stewards can mutate entries; reads are permissionless.
- Emits detailed events on every write (`ConfigUpdated(module, key, oldValue, newValue, actor)`).
- Provides helper views for frequently accessed settings (e.g., `getSwapPool()`, `getTreasury()`).

### 4.2 Qerun.sol (QER token)
- ERC-20 implementation with fixed 111,111,111 supply ceiling; mint/burn functions require governance authorization.
- Null-address transfers blocked at the contract level; emits explicit errors for misuse.
- Hooks to lock collateral when users enter borrowing products (future phases), ensuring compatibility with the reserve mechanism.
- Maintains metadata for UI (symbol, decimals, version) and exposes governance modifier to restrict sensitive functions.

### 4.3 Swap / DEX Module
- MVP supports one base asset (configured in `StateManager`).
- Pricing logic can start with a simple constant-product pool seeded by treasury liquidity.
- Integrates with protocol fee policy; fees routed to treasury or staking module once live.
- Includes pause switch for emergency halts and slippage-checked swap functions to protect users.

### 4.4 Portfolio Manager
- Tracks deposits/withdrawals of backing assets; each asset entry stores type, amount, valuation metadata, and custodian.
- Publishes aggregate metrics (total reserve value, asset mix) for dashboards via events.
- Enforces role-based access: only authorised stewards/DAO can register or move assets.
- Designed for extension: future modules can plug in valuation oracles or automated rebalancing.

### 4.5 Governance & Control Layer
- Timelock contract queues sensitive transactions (minting, config updates) and enforces delay before execution.
- Multi-signature safe holds ultimate admin role during bootstrap; DAO voting contract replaces it as trust matures.
- Emergency pause/guardian keys exist for MVP but are transparently published in `StateManager` for accountability.

## 5. Off-Chain Interfaces & Services
### 5.1 Qerun Portal (Web)
- Next.js app compiled to static assets and pinned to IPFS; ENS (e.g., `qerun.eth`) points to the content hash.
- Features: dashboard for supply/reserve metrics, swap UI, governance timeline, documentation hub.
- Fetches on-chain data via JSON-RPC or community-maintained indexers; uses `StateManager` to discover contract addresses.

### 5.2 Wallet Tracker & Alerts
- Optional service subscribing to contract events (withdrawal requests, timelock queue, reserve movements).
- Sends push/email/webhook notifications to opt-in users before withdrawals execute, aligning with MVP promises.
- Can run as lightweight serverless functions; publishes public RSS/JSON feeds for transparency.

### 5.3 Ops Tooling
- CLI scripts for deployments (Foundry/Hardhat) and recovery runbooks.
- Admin dashboards aggregating metrics from Portfolio Manager and Swap contract event logs.
- GitHub Actions pipelines for lint/test/deploy-to-IPFS tasks.

## 6. Key Flows
1. **Acquire QER**
   - User visits portal, connects wallet, reads swap pool address from `StateManager`.
   - Frontend estimates price via AMM quote; user confirms swap transaction.
   - Swap contract executes trade, emits `SwapExecuted`, updates pool balances; Portfolio Manager updates reserves if needed.

2. **Backing asset update**
   - Steward submits `registerAsset` via Portfolio Manager; transaction routed through timelock/multi-sig.
   - After delay, asset recorded; event triggers dashboard refresh.

3. **Governance action**
   - Proposal created in DAO module referencing desired change (e.g., new swap fee).
   - Once approved, timelock queues the transaction; alerts notify community.
   - After timelock expiry, executor calls the queued action; StateManager/QER contracts receive updates.

4. **Portal deployment**
   - CI builds static site, pins to IPFS (Pinata + Web3.Storage) and optionally Arweave.
   - ENS content hash updated via multi-sig; monitoring verifies propagation.

## 7. Data & State Management
- **On-chain:** All critical state lives in the contracts (token balances, reserves, configs, proposals). Immutable logs provide audit trail.
- **Off-chain caches:** Indexers maintain derived views (e.g., USD valuations, historical swaps). They must be reproducible from on-chain data.
- **Secrets & keys:** Multi-sig hardware wallets for stewards; no custodial secrets stored server-side.

## 8. Deployment Topology
- **Networks:**
  - Local development: Anvil/Hardhat node with deterministic seed data.
  - Testnet: Sepolia or Holesky for staging; contracts deployed via CI and documented.
  - Mainnet: Production release after audits and governance sign-off.
- **Hosting:**
  - IPFS pinning with two providers + optional static mirror (e.g., Vercel/Cloudflare) as read-only backup.
  - ENS records managed through timelock-controlled multi-sig.
- **Configuration:** `StateManager` seed script populates addresses post-deployment; portal reads from a JSON manifest generated by CI.

## 9. Security & Privacy
- Formal threat model covers contract exploitation (re-entrancy, oracle manipulation), governance capture, and front-end spoofing.
- Critical contracts reviewed internally + external auditors before mainnet release.
- Bug bounty launched alongside MVP to incentivise responsible disclosure.
- No PII storage; alerts rely on wallet-address subscriptions or opt-in email hashed/managed off-chain.
- Timelocks, multi-sig approvals, and withdraw alerts offer layered defence against treasury drain.

## 10. Observability & Operations
- Contract events streamed into monitoring dashboards (e.g., Dune queries, custom Grafana).
- Automated watches on timelock queues, reserve balances, and swap slippage trigger PagerDuty/Discord alerts.
- IPFS pin monitor verifies availability; ENS resolver checks run daily.
- Incident runbook defines steps for pausing contracts, informing the community, and rolling back deployments.

## 11. Scalability & Performance
- Contracts optimised for expected low-to-mid throughput; swap operations rely on established AMM patterns.
- Event-driven architecture allows adding more listeners/indexers without contract changes.
- Portal fetches aggregate data via cacheable endpoints to reduce RPC load.
- Future roadmap: horizontal scale indexers, layer-2 deployment exploration, off-chain computation hooks.

## 12. Testing & Quality Assurance
- Foundry/Hardhat unit tests for each contract covering success and failure paths.
- Integration tests exercising cross-contract flows (minting, swapping, governance updates).
- Simulation of adverse scenarios (oracle deviation, queue flooding) before deploying changes.
- Frontend end-to-end tests (Playwright) verifying critical user journeys (connect wallet, swap, read dashboard).
- CI enforces linting, style, and minimum coverage thresholds.

## 13. Risks, Assumptions, Decisions
- **Fixed supply assumption:** No minting beyond cap; if economic conditions change, DAO must amend constitution + contracts.
- **Single-asset swap MVP:** Reduces complexity but limits liquidity options; plan for multi-asset support in Phase 2.
- **Oracle dependency:** Initial version may rely on trusted price feed (Chainlink or steward-signed). Document failover procedures.
- **Bootstrap governance:** Temporary guardian keys exist; clear timeline to transition to on-chain voting documented in constitution.
- **IPFS availability:** Requires redundant pinning and regular verification to avoid site disappearance.

## 14. Roadmap Alignment & Next Steps
- Finalise detailed specs for each contract (issues #16–#19) and align code scaffolding with this architecture.
- Produce sequence diagrams for key flows (swap, governance queue, reserve update) and attach to repo once ready.
- Stand up testnet deployments with telemetry to validate observability design.
- Expand documentation into component-specific READMEs as implementation begins.
