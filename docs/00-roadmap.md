# EduInsight V2 — Roadmap & Docs Index

Follow this order. Do not skip ahead to coding before Phase 4 docs are
done.

## Phase 0 — Planning (Week 1, no coding)
- [x] [01 — Problem Statement](./01-problem-statement.md)
- [x] [02 — Requirement Analysis](./02-requirements.md)
- [x] [03 — Market Research Summary](./03-market-research-summary.md) — index table linking to all research below
  - Tier 1 — Basic form tools:
    - [MARKET-GAP/03a — Google Forms](./MARKET-GAP/03a-market-research-google-forms.md)
    - [MARKET-GAP/03b — Microsoft Forms](./MARKET-GAP/03b-market-research-microsoft-forms.md)
    - [MARKET-GAP/03c — Moodle Feedback](./MARKET-GAP/03c-market-research-moodle-feedback.md)
    - [MARKET-GAP/03d — Canvas LMS](./MARKET-GAP/03d-market-research-canvas-lms.md)
  - Tier 2 & 3 — the important gaps:
    - [MARKET-GAP/03e — AI-Native Competitors](./MARKET-GAP/03e-market-research-ai-native-competitors.md) ⚠️ closes the "no AI competitor" gap
    - [MARKET-GAP/03f — India ERP/NAAC-NBA Players](./MARKET-GAP/03f-market-research-india-erp-players.md) ⚠️ critical for India market
  - [MARKET-GAP/03g — Market Gap Analysis & Project Review](./MARKET-GAP/03g-market-gap-analysis-and-project-review.md) — final positioning + recommendations
- [x] [04 — Feature List & Tech Stack](./04-feature-list-and-tech-stack.md)

## Phase 1 — Database Design (Week 2)
- [x] [05 — Database Design](./05-database.md)
- [ ] Draw full ER diagram in draw.io → save to `database/er-diagram.png`

## Phase 2 — API Design
- [x] [06 — API Design](./06-api-design.md)
- [ ] Convert to formal spec → `api/openapi.yaml`

## Phase 3 — System Design
- [x] [07 — System Design](./07-system-design.md)
- [ ] Draw architecture diagram → `architecture/system-design.png`

## Phase 4 — Folder Structure
- [x] [08 — Folder Structure](./08-folder-structure.md)

## Phase 5 — Coding Starts: Authentication
- [x] [09 — Authentication Module](./09-authentication.md)

## Phase 6 — Student Module
*(create `docs/13-student-module.md` when you reach this phase — same
template as Feedback/AI docs: Scope, Entities, Flow, Checklist, Done
Criteria)*

## Phase 7 — Faculty Module
*(create `docs/14-faculty-module.md`, same template)*

## Phase 8 — Feedback Module
- [x] [10 — Feedback Module](./10-feedback-module.md)

## Phase 9 — AI Module
- [x] [11 — AI Module](./11-ai-module.md)

## Phase 10 — Dashboard
*(create `docs/15-dashboard.md`, same template)*

## Phase 11–12 — Docker & AWS
- [x] [12 — Deployment](./12-deployment.md)

## Phase 13 — Open Source / GitHub polish
- [ ] Write root `README.md` (overview, tech stack, setup instructions,
      screenshots)
- [ ] Add architecture decision records in `architecture/decisions/`
      (start with `001-monolith-vs-microservices.md`)
- [ ] License file (MIT recommended for portfolio projects)

---

## Open Item — Pending Decision
Per the market gap analysis (`MARKET-GAP/03g`), consider revisiting
`01-problem-statement.md` to:
- Update the competitor table to include Tier 2 (Explorance, Watermark,
  Qualtrics, Perspective AI) and Tier 3 (Camu, EduSys, LearnQoch) players,
  not just basic form tools.
- Reframe positioning as **"India-first, accreditation-aware feedback
  intelligence platform"**.
- Add a NAAC/NBA evidence-export feature to the roadmap (future phase).

## Status: Phase 0 Complete ✅
- [x] Problem Statement
- [x] Requirement Analysis
- [x] Market Research (all tiers, including AI-native and India ERP competitors)
- [x] Feature List & Tech Stack

Next: Database Design (`05-database.md`) → draw ER diagram in draw.io.
