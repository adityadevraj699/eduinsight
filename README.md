# EduInsight — AI Powered Feedback Intelligence Platform

> College feedback systems collect data. EduInsight turns that data into
> actionable insight for Faculty, HODs, and Admins — using AI sentiment
> and theme analysis on top of structured + free-text feedback.

## 📌 Problem
Existing tools (Google Forms, Moodle Feedback, Canvas) collect feedback
but stop at raw numbers. Faculty get a "4.2/5" average with no idea what
to actually improve. EduInsight closes that gap with an AI analysis layer.

Full write-up: [`docs/01-problem-statement.md`](./docs/01-problem-statement.md)

## 🏗️ Architecture
Modular Monolith (Spring Boot) — package-by-feature, designed so any
module (e.g. `ai`) can be extracted into its own service later without a
rewrite.

```
React (TS) → Spring Boot → Redis (cache) → PostgreSQL
                    ↓
                  Kafka → AI Service (Spring AI / LLM) → Email/Notification
```

Full diagram & reasoning: [`docs/07-system-design.md`](./docs/07-system-design.md)

## 🧰 Tech Stack

| Layer | Tech |
|---|---|
| Backend | Java 21, Spring Boot, Spring Security, Spring AI |
| Frontend | React, TypeScript, Tailwind CSS |
| Database | PostgreSQL |
| Cache | Redis |
| Queue | Kafka |
| Deployment | Docker, AWS |

## 📂 Project Structure
```
EduInsight/
├── backend/        # Spring Boot modular monolith
├── frontend/        # React + TypeScript
├── database/         # ER diagrams, schema
├── docker/            # docker-compose for local dev
├── architecture/       # system design diagrams + ADRs
├── api/                # OpenAPI spec
├── docs/                # full design documentation (read this first)
└── monitoring/           # Prometheus/Grafana configs
```

Full explanation: [`docs/08-folder-structure.md`](./docs/08-folder-structure.md)

## 📖 Documentation
All planning and design decisions are documented **before** any code was
written — see [`docs/00-roadmap.md`](./docs/00-roadmap.md) for the full
index (problem statement → requirements → database design → API design →
system design → module-by-module build docs).

## 🚀 Getting Started (once code exists)
```bash
git clone https://github.com/<your-username>/eduinsight.git
cd eduinsight
docker compose -f docker/docker-compose.yml up
```
Backend: `http://localhost:8080` · Frontend: `http://localhost:3000`

## 🗺️ Roadmap Status
- [x] Phase 0 — Planning & Requirements
- [x] Phase 1 — Database Design
- [x] Phase 2 — API Design
- [x] Phase 3 — System Design
- [ ] Phase 5 — Authentication Module (in progress)
- [ ] Phase 6–10 — Core modules
- [ ] Phase 11–12 — Docker & AWS deployment

## 📄 License
MIT
