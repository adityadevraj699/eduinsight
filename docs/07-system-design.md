# 07 — System Design

## High-Level Flow

```
                     ┌────────────┐
                     │   React    │  (TypeScript + Tailwind)
                     │  Frontend  │
                     └─────┬──────┘
                           │ REST (JSON, JWT auth)
                           ▼
                  ┌──────────────────┐
                  │   Spring Boot    │  ← Modular Monolith
                  │   (Backend API)  │
                  └───┬─────────┬────┘
                      │         │
           read-heavy │         │ write feedback
                      ▼         ▼
              ┌───────────┐  ┌──────────────┐
              │   Redis   │  │  PostgreSQL  │
              │  (cache)  │  │  (primary DB)│
              └───────────┘  └──────────────┘
                      │
                      │ publish event: "feedback.submitted"
                      ▼
                ┌───────────┐
                │   Kafka   │
                └─────┬─────┘
                      │ consume
                      ▼
              ┌────────────────┐
              │   AI Service    │  (Spring AI → LLM)
              │ (async consumer)│
              └────────┬────────┘
                        │ writes AI_Report → PostgreSQL
                        │ publishes "report.ready" event
                        ▼
                ┌───────────────┐
                │ Email Service  │ → notifies Faculty/HOD
                └───────────────┘
```

## Why Async (Kafka) for AI?
- Feedback submission is a **user-facing, latency-sensitive** action — it
  must respond fast regardless of AI processing time.
- LLM calls can be slow (seconds) and occasionally fail/rate-limit — Kafka
  gives us retry, backpressure, and decoupling for free.
- If the AI service is down, feedback still gets saved; analysis catches
  up later (**graceful degradation** — see NFR in `02-requirements.md`).

## Caching Strategy (Redis)
- Cache dashboard aggregation queries (`/api/dashboard/*`) with a TTL
  (e.g. 5–10 min) since they're read-heavy and don't need real-time
  freshness.
- Invalidate/refresh cache when a new `AI_Report` is generated for that
  subject/cycle.

## Security Flow
```
Login → Spring Security validates credentials → JWT issued
        (JWT contains: userId, role, expiry)
Every request → JWT filter validates token → SecurityContext populated
              → @PreAuthorize checks role for endpoint (RBAC)
```

## Deployment View (Phase 12 — AWS)
```
Route53 → ALB → ECS/EC2 (Spring Boot container)
                        │
                ┌───────┼────────┐
                ▼                ▼
             RDS (Postgres)   ElastiCache (Redis)
                │
          MSK (managed Kafka) or self-hosted on EC2
```

## Key Design Decisions to Document as ADRs
1. Monolith vs Microservices → chose Modular Monolith (see
   `architecture/decisions/001-monolith-vs-microservices.md`)
2. Sync vs Async AI processing → chose Async via Kafka
3. PostgreSQL vs MongoDB → chose PostgreSQL (relational data model)
4. Redis caching strategy → TTL + event-based invalidation
