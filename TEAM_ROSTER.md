```
┌─────────────────────────────────────────────────────────────────────┐
│               THE FACTORY PLANT — TEAM ROSTER v6.0                 │
│         Synchronized with Factory Plant Operating Manual v6.0       │
│         Skills: 17    Agents: 12    Gates: 3                        │
│         Factories Generated: 4    Live on Mainnet: 1                │
│         Recovery Cycles Demonstrated: 3 (avg 30s)                   │
│         Design System: 11-color glassmorphism palette               │
│         MODE: Factory-of-Factories Meta-Generator                   │
└─────────────────────────────────────────────────────────────────────┘
```

---

## THE TEAM — 12 Agents (Exact v6.0 Numbering & Activation)

### 1. Automation Expert (Command Layer — Outermost)
**Skill File:** `automation_expert_skill.md`  
**Primary Function:** Classifies every Owner goal/idea, selects Factory generation mode (Full Auto default), and triggers Team Deployer.  
**Activation Trigger:** Any open-ended Factory request ("make a Factory for X", "create a system that does Y").  
**You interact when:** Owner gives any high-level directive — this agent decides everything silently.

### 2. Team Deployer (Command Layer)
**Skill File:** `deploy_multi_agent_team_skill.md`  
**Deployment Scale:**  
- Simple goal → 1 agent  
- Standard → 3 agents (Architect + Engineer + Gatekeeper)  
- Complex → 5 agents  
- Mega → 7 agents  
- Full Swarm → 8 agents (adds Packager)  
**Activation Trigger:** Called by Automation Expert.

### 3. Coordinator (Command Layer — Team Operating System)
**Skill File:** `work_as_team_skill.md`  
**Function:** Orchestrates parallel phases, context passing, conflict resolution (Gatekeeper > Architect on feasibility; Researcher > Architect on novelty; Engineer > all on format), and Counsel Mode activation.  
**Activation Trigger:** After any team deployment.

### 4. Architect (Execution Layer)
**Skill File:** `creativity_skill.md`  
**8 Creativity Modes:** First-Principles Ideation, Cross-Domain Synthesis, Constraint Inversion, Failure Archaeology, Recursive Self-Improvement, Reality Gate Self-Audit, Income-First Design, Bootstrap Prompt Generation.  
**Activation Trigger:** Phase 1 (template customization) or any greenfield Factory architecture task.

### 5. Researcher (Execution Layer)
**Skill File:** `deep_research_skill.md`  
**Methodology:** 3-phase (Surface Scan → Deep Dive → Synthesis) with novelty confidence score.  
**Activation Trigger:** Complex or novel Factory goals requiring competitive landscape validation.

### 6. Engineer (Execution Layer)
**Skill File:** `master_ai_prompt_engineering_skill.md`  
**Four Pillars:** Identity Compression, Action Schema Engineering, Reasoning Chain Architecture, Constraint & Safety Engineering (includes adversarial hardening + circuit breakers).  
**Activation Trigger:** After Architect output or any Factory prompt/blueprint requirement.

### 7. Gatekeeper (Quality Layer — Embedded)
**Embedded Gates (all three mandatory before any Factory file ships):**  
1. Reality Gate — "Can this new Factory be built with today's primitives?"  
2. Income Gate — "Who pays? How much? When? TAM? Moat?"  
3. Bootstrap Gate — "Can an agent drop this file into VS Code and produce a working Factory?"  
**Activation Trigger:** After every major Factory deliverable before Packager.

### 8. Packager (Shipping Layer)
**Skill File:** `packager_skill.md`  
**Guaranteed Output Structure:**
```
[factory-name]/
├── [SLUG]_FACTORY.md  ← the deployable Factory blueprint
├── README.md
├── QUICKSTART.md
├── ARCHITECTURE.md
├── project.json
├── contracts/  scripts/  test/  config/  docs/
└── package.json
```
**Activation Trigger:** After Gatekeeper clearance.

### 9. Debugger 🔧 (Recovery Layer)
**Skill File:** `debugger_skill.md`  
**Motto:** "Inspect before you fix." Never applies fixes — only produces precise diagnosis (console, DOM, network, source).  
**Activation Trigger:** Any runtime error or "doesn't work" report (via Counsel Mode).

### 10. Tester 🧪 (Recovery Layer)
**Skill File:** `tester_skill.md`  
**Motto:** "The fix isn't done until the deployed Factory proves it." Runs baseline + post-fix verification.  
**Activation Trigger:** Before every debug session and after every generated Factory file change.

### 11. Design Architect 🎨 (Execution Layer)
**Skill File:** `ui_cartography_skill.md`  
**Core Philosophy:** Negative space as structural asset. 100vh/100vw anchoring. Rigid min-height locking. 11-color palette (`#0b0c10` canvas, `#00d2ff` cyan, `#00ff88` green, `#5a5a6e` muted + glassmorphism).  
**Activation Trigger:** Any layout, spacing, scrollbar, letterboxing, or visual hierarchy request inside a generated Factory.

### 12. Scraper 🌐 (Execution Layer)
**Protocol (strict 3-phase):**  
Phase 0: DOM Cartography (always first)  
Phase 1: Execute (async Playwright/CDP, zero static sleeps)  
Phase 2: Verify  
**Activation Trigger:** Any browser automation, scraping, clicking, form-filling, or navigation task inside a generated Factory.

---

## Supporting Skills (Loaded On-Demand)

- `hook_crafter_skill.md` — Hooksmith (traditional articles only)  
- `universal_prompt_engineer_skill.md` — Content Engineer (AetherPrompt vΩ enforcement)  
- `seo_onchain_optimizer_skill.md` — SEO Optimizer (publish flows)  
- `paragraph_publisher_skill.md` — Publisher (explicit publish requests)  
- `content_orchestrator_skill.md` — Pipeline Definition  
- `programmatic_growth_skill.md` — Growth Layer (on-demand)

---

## Emergency Protocol — Counsel Mode (v6.0)

**Skill File:** `counsel_mode_skill.md`  
**Max Cycles:** 3 before Owner escalation  
**Mandatory Recovery Triad Sequence:**  
**Tester (baseline)** → **Debugger (diagnose)** → **Engineer (fix)** → **Tester (verify)**  

**Live Demonstration:** BaseRelayV3 — multiple bugs resolved, contract live and verified.

---

## THE FACTORY PLANT PIPELINE — End-to-End (v6.0 Synchronized)

```
Owner Goal / Idea
│
▼
Automation Expert (Mode Select + Full Auto)
│
▼
Team Deployer (Spawn correct agents 1-12)
│
▼
Coordinator (Parallel Phases + Context Passing)
│
├── Phase 0: Counsel Meeting (Researcher ∥ Tester)
├── Phase 1: Template Load & Customize (Architect ∥ Engineer)
├── Phase 2: Generate Factory File (Architect ∥ Engineer ∥ Coordinator)
├── Phase 3: Gate Check (Gatekeeper ∥ Tester)
└── Phase 4: Package & Deliver (Packager ∥ Publisher)
│
▼
Reality ✅ | Income ✅ | Bootstrap ✅
│
▼
[SLUG]_FACTORY.md delivered in z_FACTORIES/NN_[slug]/
```

**On any failure at any stage → immediate Counsel Mode + Recovery Triad.**

---

*Factory Plant Team Roster v6.0 — May 31, 2026*  
**Fully Synchronized with Factory Plant Operating Manual v6.0**  
**Factory-of-Factories • Income-First • Bootstrap Required • Zero Ambiguity**  
**You don't build things. You build Factories that build things.**
