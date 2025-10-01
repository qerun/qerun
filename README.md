# QERUN

**QERUN** is a community-driven crypto ecosystem designed to provide a safe, transparent, and flexible way for people to invest, grow, and transfer value.  
At its core, QERUN introduces the **QER token**, backed by on-chain assets and governed by a DAO to ensure fairness and adaptability.

---

## 🚀 Vision
QERUN aims to become:
- A **safe place** for people to hold and grow value.
- A **community hub** for governance, investment, and innovation.
- A **transparent protocol** where supply, backing, and decisions are on-chain and verifiable.

---

## 🔮 Roadmap (High Level)
1. **MVP** – QER token + basic Swap (buy with WETH/stablecoin).  
2. **Portfolio Manager** – DAO-driven multi-asset backing.  
3. **DAO Governance** – Minting, cap, and allocation controlled by community vote.  
4. **Expansion** – Integrations, advanced financial tools, new assets.  

## 🛠️ Development Workflow
- `main` stays release-ready; CI/CD and deployments should target this branch only.  
- Day-to-day work happens on `dev`. Create feature branches from `dev` and open PRs back into `dev` for review.  
- Promote changes to `main` through a reviewed PR from `dev → main` once the release cut is stable.  
- Avoid direct pushes to either `main` or `dev`; use pull requests so reviews and automated checks gate every change.  
- Keep your local repo in sync with `origin/dev` before starting new work: `git checkout dev && git pull origin dev`.  
