# Qerun Tokenomics (Draft v1)

## 1. Introduction & Purpose
The Qerun token (QER) is the native asset of the Qerun ecosystem. Its purpose is to:
- Enable community governance over protocol upgrades and parameters.
- Provide utility within the ecosystem (staking, reputation, access).
- Align incentives between users, contributors, and the protocol.

Qerun follows a **safety-first tokenomics design**: prioritizing trust, capital protection, and sustainable growth.

---

## 2. Token Supply
- **Total Supply**: Fixed at 111,111,111 QER at launch; minted to the Treasury at deployment.
- **Minting/Burning**: No minting or burning functions exist in the token contract; null-address transfers are blocked.
- **Distribution (initial outline)**:
  - Community & Ecosystem: majority allocation.
  - Team & Contributors: vesting-based, long-term locked.
  - Treasury: reserved for future development.
  - Liquidity Incentives: gradual release.

---

## 3. Utility & Use Cases
- **Governance**: vote on proposals and parameters.
- **Staking**: locking QER enhances governance power and unlocks ecosystem benefits.
- **Reputation**: QER staking serves as a credibility signal within the community.
- **Borrowing**: QER holders can borrow stablecoins against their holdings (see Section 4).
- **Access**: certain advanced features and perks require QER.
- **Base Token Utility**: QER is not only for voting but also serves as the foundational asset for multiple ecosystem functions.

---

## 4. Borrowing & ETH Reserve Mechanism (Future)
- Borrowing against QER is a future feature subject to governance design and audits.
- Any borrowing model will be implemented in separate modules/contracts (not in the QER token), governed via `StateManager`-wired policies.
- The DAO will set LTV, reserve routing, and liquidation parameters through governance modules and timelock.

---

## 5. Incentive Design
- **Liquidity Incentives**: rewards for providing liquidity to pools.
- **Long-term Holding**: staking/locking rewards for users who commit to QER.
- **ETH Growth Loop**: ETH purchased from borrowing creates double exposure to ecosystem growth.
- **Fixed Supply Incentives**: with a fixed supply and no minting/burning, incentives focus on value accrual and ecosystem participation rather than inflationary rewards.

---

## 6. Governance & Safeguards
- Community DAO will progressively control major decisions.
- Sensitive parameters and module pointers are updated via `StateManager`, with optional STATICCALL on policy modules to ensure they remain read-only.
- Governance can vote on borrowing ratios, reserve management, and incentive structures in future phases.

---

## 7. Security & Risk Mitigation
- Conservative borrowing (LTV DAO).
- Clear liquidation rules to prevent systemic risk.
- Insurance/Treasury allocation to cover bad debt.
- Reliance on reliable oracles for pricing QER and ETH.

---

## 8. Roadmap
- **Phase 1**: Launch with governance, staking, and basic utility.
- **Phase 2**: Enable borrowing against QER with ETH reserve accumulation.
- **Phase 3**: Expand to advanced features, integrations, and deflationary mechanics.

---

## 9. Conclusion
Qerun’s tokenomics emphasize **trust, sustainability, and growth**. By combining QER collateralization with ETH reserve accumulation, the system offers users both security and upside potential, while reducing active supply and strengthening the ecosystem over time.