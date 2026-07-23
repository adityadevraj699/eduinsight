# 04 — Feature List & Tech Stack

## Multi-Tenancy Architecture Decision
EduInsight is a **multi-tenant SaaS**: each university gets its own
**subdomain** (e.g. `xyzcollege.eduinsight.com`) and its own **separate
database** (Silo Model).

- Subdomain resolves the tenant **before** login happens — the request
  hits `xyzcollege.eduinsight.com`, a `TenantResolverFilter` reads the
  subdomain, looks up which database connection to use (via a small
  central **Tenant Registry** table/service that maps `subdomain →
  db_connection_string`), then routes the request to that university's
  database. Login (`email + password`) only ever gets checked against
  **one** database — the one already resolved from the subdomain. This is
  what avoids the "check 100 databases" problem.
- New module needed for this: **Tenant Provisioning / Super Admin**
  (see below) — this is the platform owner's own control panel, separate
  from any single university's Admin.

## Feature List (Modules)

### Core Academic Modules (per-university, inside each tenant DB)
```
Authentication
University
Department
Faculty
Student
Subjects
Feedback
AI
Reports
Dashboard
Notifications
```

### Platform-Level Modules (NEW — these sit above all universities)
These are missing from the original list because they belong to
**you (the platform owner)**, not to any single university.

```
SuperAdmin          — platform owner's own dashboard, manages ALL universities
TenantProvisioning  — onboarding flow: create subdomain + spin up new DB automatically
Subscription        — plan selection, upgrade/downgrade, trial period
Billing             — invoicing, payment gateway integration, monthly/yearly cycles
UsageMetering       — track usage against plan limits (students, AI calls, storage)
FeatureFlags        — plan-based feature gating (which module is ON for which tier)
AuditLog            — platform-wide activity log across all tenants (compliance)
```

Each of these will become its own package/module in the backend
(package-by-feature) and its own module folder in the frontend. Note:
`SuperAdmin`, `Subscription`, `Billing`, `TenantProvisioning` live in a
**separate platform-level database** (not inside any university's tenant
DB) — this is your control plane.

## Subscription Plans

| Plan | Billing | Target |
|---|---|---|
| **Free** | ₹0 | Trial / very small colleges, or startup-phase user acquisition |
| **Middle** | Monthly & Yearly | Mid-size colleges wanting AI insights |
| **Pro** | Monthly & Yearly | Large universities / multi-department, full feature set |

> Yearly billing should carry a discount (commonly ~15-20% off monthly ×
> 12) to incentivize annual commitment — standard SaaS practice, decide
> exact % later.

## Feature-to-Plan Mapping

| Feature | Free | Middle | Pro |
|---|:---:|:---:|:---:|
| Authentication (RBAC) | ✅ | ✅ | ✅ |
| University/Department/Faculty/Student mgmt | ✅ (limited count) | ✅ | ✅ (unlimited) |
| Subject & enrollment mgmt | ✅ | ✅ | ✅ |
| Feedback collection (structured + free-text) | ✅ | ✅ | ✅ |
| Basic averages/charts (no AI) | ✅ | ✅ | ✅ |
| **AI sentiment analysis** | ❌ | ✅ | ✅ |
| **AI theme extraction** | ❌ | ✅ | ✅ |
| **AI natural-language summary** | ❌ | ✅ | ✅ |
| Faculty dashboard | ✅ (basic) | ✅ (AI-enhanced) | ✅ (AI-enhanced) |
| HOD cross-faculty comparison dashboard | ❌ | ✅ | ✅ |
| Admin university-wide analytics | ❌ | Limited | ✅ (full) |
| Report export (PDF/CSV) | Basic CSV only | PDF + CSV | PDF + CSV + scheduled auto-export |
| NAAC/NBA evidence export | ❌ | ❌ | ✅ |
| Email notifications | ✅ (basic) | ✅ | ✅ + SMS/WhatsApp (optional) |
| Student/user limit | Capped (e.g. 200) | Capped (e.g. 2000) | Unlimited |
| Data retention | 6 months | 2 years | Unlimited |
| Support | Community/email | Email (priority) | Dedicated support |

> **Note on Free tier AI**: keeping AI fully locked out of Free is the
> standard approach — it protects your LLM API cost (the most expensive
> line item) while still letting Free users experience the core feedback
> loop. If you're in an early growth/startup phase and need Free-tier
> adoption fast, a common middle path is: give Free users a **capped
> number of AI analyses per month** (e.g. 1 free AI report per semester)
> instead of zero — enough to taste the value, not enough to replace
> paying for Middle/Pro. Decide this based on your growth strategy, not
> as a permanent technical constraint — it's just a `FeatureFlags` config
> value, easy to change later.

## Tech Stack (Final)

### Backend
| Tech | Purpose |
|---|---|
| Java 21 | Language (virtual threads help with async I/O) |
| Spring Boot | Core framework |
| Spring Security | Auth + RBAC |
| Spring AI | Wrapper to call LLM for feedback analysis |
| Flyway | Database migrations (run per-tenant DB on provisioning) |

### Frontend
| Tech | Purpose |
|---|---|
| React | UI library |
| TypeScript | Type safety |
| Tailwind CSS | Styling |

### Database & Infra
| Tech | Purpose |
|---|---|
| PostgreSQL | One primary DB per tenant (university) + one platform-level control-plane DB |
| Redis | Caching (dashboard queries, session/rate-limit) + tenant-registry cache (subdomain → DB mapping) |
| Kafka | Async queue — feedback → AI processing pipeline |
| Docker | Containerization |
| AWS | Deployment (EC2/ECS + RDS + S3) |

### Billing & Payments (NEW)
| Tech | Purpose |
|---|---|
| Razorpay Subscriptions (or Stripe Billing) | Recurring monthly/yearly subscription charges |
| Webhook handler | Sync payment success/failure/renewal/cancellation events back into `Subscription`/`Billing` modules |

## Why These Choices (short justification — expand in ADRs)
- **PostgreSQL over MongoDB**: data is highly relational (University →
  Department → Faculty/Subject → Feedback), foreign keys and joins matter.
- **Kafka over direct sync call**: AI analysis shouldn't block feedback
  submission response time; also allows retry/replay if AI service fails.
- **Redis**: dashboard reads are frequent and repetitive (same HOD checking
  same report) — cache aggressively, invalidate on new feedback cycle.
  Also caches the subdomain → tenant-DB lookup so every request doesn't
  hit the control-plane DB.
- **Modular Monolith over Microservices**: team size = 1 (or small), scale
  requirement doesn't justify microservices overhead yet. See
  `architecture/decisions/001-monolith-vs-microservices.md`.
- **Separate DB per tenant (Silo Model)**: chosen over shared DB because
  subdomain-based routing resolves the tenant before any query runs,
  avoiding cross-database lookups at login. Trade-off to note in an ADR:
  this means migrations (Flyway) must run against every tenant DB on
  deploy, and provisioning a new university requires an automated
  `TenantProvisioning` step (create DB → run migrations → register
  subdomain) rather than just inserting a row.
