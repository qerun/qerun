# Qerun Constitution Draft Notice

*This document is a working draft of the Qerun Constitution. It outlines the foundational principles and governance framework for the Qerun community and is subject to ongoing refinement.*

---

## Preamble

We, the members of Qerun, establish this Constitution to foster a decentralized and resilient digital economy that empowers individuals without requiring state-issued identification. Our community values privacy, credibility, and mutual trust, enabling participation based on self-sovereign identity and collective accountability. Together, we commit to building an equitable and innovative ecosystem that adapts to the evolving needs of its members.

---

## Section 1: Core Values & Principles

1. **Self-Sovereign Participation**  
   Every member retains full control over their identity and data, participating freely without reliance on centralized authorities or state-issued identification.

2. **Accountability Through Transparency**  
   All governance processes, decisions, and financial activities shall be open and accessible to the community, fostering trust and informed participation.

3. **Fair Participation**  
   Mechanisms will be in place to ensure equitable access and influence, preventing dominance by any single party or group.

4. **Inclusivity**  
   Qerun welcomes diverse perspectives and backgrounds, ensuring accessibility and respect for all members.

5. **Innovation**  
   We encourage continuous improvement and creative solutions to advance the community’s mission and governance.

---

## Section 2: Membership

### Rights and Responsibilities

- Members have the right to participate in governance, propose initiatives, and access community resources.
- Members are responsible for upholding the community’s values, adhering to the Code of Conduct, and contributing positively.
- Participation requires no state-issued identification, preserving privacy and self-sovereignty.

### Pioneers

- Pioneers are founding or early members recognized for their contributions to Qerun's establishment and growth.  
- Pioneer perks and recognitions include:  
  - Guaranteed allowlist access to new vaults, products, and pilot programs before wider release.  
  - A 1.25x governance voting multiplier during the first year of DAO operations (subject to renewal vote thereafter).  
  - A commemorative Pioneer NFT that unlocks exclusive community channels, events, and reputation badges.  
  - Eligibility for rotating advisory seats that help shape quarterly priorities and stewardship guidelines.  
- Perks are reviewed annually by the DAO to ensure they stay aligned with community values and avoid entrenched privilege.

---

## Section 3: Governance Structure

- The Qerun DAO Treasury is managed collectively by members, ensuring transparent handling of community funds.
- Members vote on treasury allocation, directing resources to projects and initiatives aligned with Qerun’s mission.
- Proposals can be submitted by any member and are subject to community discussion and voting before implementation.
- Optional councils or committees may be established to oversee specialized areas or provide advisory support.

---

## Section 4: Tokenomics & Value Exchange

- Qerun tokens represent membership, governance rights, and value exchange within the ecosystem.
- Token staking mechanisms enable participation in governance and incentivize long-term commitment.
- A rewards vault is maintained to recognize contributions and foster continued engagement.
- Credibility applications leverage tokenomics to build trust and reputation without centralized control.

---

## Section 5: Community Contributions

- Members contribute through development (smart contracts, frontend, audits), operations (infrastructure, security, treasury management), governance participation (proposal drafting, moderation), community growth (education, support, partnerships), and creative work (content, design, product research).
- Contribution recognition and rewards draw from the DAO rewards vault and other approved budgets; exact allocations and payout mechanics are decided by community vote and revisited during periodic treasury reviews.
- Contributors are encouraged to document deliverables and measurable impact so rewards remain transparent and auditable.

---

## Section 6: Resource Allocation

- Allocation of resources is fully governed by the community, ensuring alignment with collective priorities.
- Governance controls budget distribution, project funding, and operational expenses.  
- Each treasury cycle (recommended quarterly), the DAO considers an allocation proposal that outlines funding for core development, operations/security, community programs, and a contingency reserve. For example, an initial framework may earmark 40% for core development (contracts, audits, engineering), 25% for operations and security (infrastructure, legal, risk mitigation), 25% for community initiatives (education, grants, ecosystem growth), and 10% for an emergency reserve. Actual ratios are decided by governance vote and may shift as priorities evolve.
- Allocation proposals must publish expected deliverables, responsible stewards, and review checkpoints. Funds are streamed or released in milestones where feasible to maintain accountability.
- Emergency expenditures outside the approved allocation require a supermajority vote or designated guardian approval, followed by retrospective disclosure to the DAO.

---

## Section 7: Code of Conduct

- Members shall engage respectfully, fostering a safe and inclusive environment.
- Harassment, discrimination, and disruptive behavior are prohibited.
- Violations are subject to community review and appropriate action.

---

## Section 8: Amendments & Evolution

- This Constitution may be amended through a transparent proposal and voting process.
- Amendments require a defined quorum and majority to ensure broad consensus.
- The document shall evolve to reflect the community’s growth and changing needs.
- A formal constitutional review and update cycle is scheduled at least once per year (or sooner if triggered by governance), during which the DAO evaluates existing clauses, collects improvement proposals, and ratifies necessary adjustments.

---

## Section 9: Transparency & Accountability

- All governance activities, financial transactions, and decision-making processes are documented and publicly accessible.
- Members hold each other accountable through open dialogue and established review mechanisms.

---

## Section 10: Partnerships & External Relations

- Qerun may engage with external organizations and communities to advance shared goals.
- Partnerships shall align with Qerun’s values and be subject to community approval.

---

## Section 11: Final Provisions

- This Constitution is designed to be resilient and adaptive, guiding Qerun’s operations and governance.  
- It serves as a foundational framework to support the community’s collective success and long-term sustainability.  
- The community commits to uphold these principles in pursuit of shared goals.  

---

## Appendix A: Roles & Permissions Glossary

- **Owner (State Manager)** – Initial steward responsible for onboarding governance structures; role expected to transition to DAO-controlled timelock/multisig.
- **Module Admin Roles** – Bytes32 identifiers (e.g., `ROLE_SWAP_ADMIN`, `ROLE_TREASURY_ADMIN`) managed via the State Manager contract; grant module-specific write permissions and can be delegated through governance proposals.
- **ID Writers** – Addresses explicitly allowlisted in State Manager to manage a specific configuration entry (e.g., updating a contract address or parameter).
- **DAO Treasury Stewards** – Accounts or committees authorized by governance to execute treasury transactions within approved budgets.
- **Guardians** – Emergency responders empowered to pause contracts or authorize urgent reallocations under strict guardrails, with mandatory post-event disclosure.

Roles are maintained on-chain via the State Manager registry; any changes (grant/revoke) must be recorded through governance-tracked transactions so off-chain documentation and code remain consistent. Governance module assignment and static-mode configuration emit events (`GovernanceModuleUpdated`, `GovernanceModuleOperationUpdated`, `GovernanceStaticUpdated`, `GovernanceStaticOperationUpdated`) to facilitate transparency and indexing.
