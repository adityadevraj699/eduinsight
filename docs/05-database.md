# 05 — Database Design

## Architecture: Two Separate Databases

Per the multi-tenancy decision (subdomain + separate DB per university),
this project has **two distinct databases**, not one:

| Database | Count | Who owns it | Purpose |
|---|---|---|---|
| **Tenant DB** (one per university) | 1 per university, N total | Each university | Academic data — feedback, students, faculty, etc. Fully isolated per tenant. |
| **Control-Plane DB** (one, platform-wide) | 1 total | You (platform owner) | Business data — which universities exist, their subdomain, their plan, billing, usage |

A request first hits the subdomain (e.g. `xyzcollege.eduinsight.com`) →
`TenantResolverFilter` looks up `Tenant` in the **Control-Plane DB** →
gets the connection string for that university's **Tenant DB** → routes
the rest of the request there. This is why control-plane entities must
never be duplicated inside a tenant DB, and vice versa.

---

## Part A — Tenant DB Entities (13) — one copy per university

```
University
Department
Faculty
Student
Subject
Course
Enrollment
Question
Feedback
Response
AI_Report
Notification
Role
Permission
```

### University
- id, name, code, address, created_at

### Department
- id, university_id (FK), name, code

### Faculty
- id, department_id (FK), user_id (FK → Auth), name, designation, employee_code

### Student
- id, department_id (FK), user_id (FK → Auth), name, roll_number, batch_year

### Subject
- id, department_id (FK), faculty_id (FK), name, code, semester, credits

### Course
> Note: `Course` groups multiple `Subjects` under a program (e.g. "B.Tech
> CSE 2024 batch"). Keep this only if you need program-level grouping;
> otherwise Subject alone may be enough for V1 — decide during ER modeling.
- id, department_id (FK), name, duration_years

### Enrollment (junction table — Student ↔ Subject)
- id, student_id (FK), subject_id (FK), semester, enrolled_at
> This was implied in the relationships diagram but not listed as its own
> entity before — adding it explicitly since Student↔Subject is many-to-many.

### Question
- id, text, type (RATING / TEXT / MCQ), category (e.g. "Teaching Quality",
  "Course Content"), is_active

### Feedback (a submission "envelope")
- id, student_id (FK), subject_id (FK), cycle_id/semester, submitted_at,
  status (SUBMITTED / ANALYZED)

### Response (individual answer within a Feedback)
- id, feedback_id (FK), question_id (FK), rating_value (nullable),
  text_value (nullable)

### AI_Report
- id, subject_id (FK), cycle_id, sentiment_score, sentiment_label
  (POSITIVE/NEUTRAL/NEGATIVE), key_themes (JSON array), summary_text,
  generated_at

### Notification
- id, user_id (FK), type, message, is_read, created_at

### Role
- id, name (STUDENT / FACULTY / HOD / ADMIN)

### Permission
- id, role_id (FK), resource, action (READ/WRITE/DELETE)

### Tenant DB Relationships
```
University (1) ──< (N) Department
Department (1) ──< (N) Faculty
Department (1) ──< (N) Student
Department (1) ──< (N) Subject
Department (1) ──< (N) Course
Faculty    (1) ──< (N) Subject       [subject taught by faculty]
Student    (N) ──< (N) Subject       [via Enrollment junction table]
Student    (1) ──< (N) Feedback
Subject    (1) ──< (N) Feedback
Feedback   (1) ──< (N) Response
Question   (1) ──< (N) Response
Subject    (1) ──< (N) AI_Report
Role       (1) ──< (N) Permission
User       (N) ──> (1) Role
```

---

## Part B — Control-Plane DB Entities (8) — one copy, platform-wide

```
Tenant
Plan
Subscription
Invoice
Payment
UsageMetric
SuperAdminUser
AuditLog
```

### Tenant
- id, university_name, subdomain (unique), db_connection_string
  (encrypted), status (ACTIVE/SUSPENDED/PENDING), created_at
> This is the registry that solves tenant resolution — subdomain →
> which Tenant DB to connect to. Looked up on every request, so cache
> this in Redis (subdomain → connection string, short TTL).

### Plan
- id, name (FREE/MIDDLE/PRO), price_monthly, price_yearly,
  feature_limits (JSON — e.g. `{"ai_enabled": true, "max_students": 2000}`)

### Subscription
- id, tenant_id (FK), plan_id (FK), billing_cycle (MONTHLY/YEARLY),
  status (ACTIVE/TRIAL/EXPIRED/CANCELLED), start_date, end_date,
  auto_renew (boolean)

### Invoice
- id, tenant_id (FK), subscription_id (FK), amount, billing_period_start,
  billing_period_end, status (PAID/PENDING/OVERDUE), issued_at

### Payment
- id, invoice_id (FK), gateway_transaction_id, gateway (RAZORPAY/STRIPE),
  amount, status (SUCCESS/FAILED/REFUNDED), paid_at

### UsageMetric
- id, tenant_id (FK), metric_type (STUDENT_COUNT/AI_CALLS/STORAGE_MB),
  value, recorded_at (snapshot per period, used to enforce plan limits)

### SuperAdminUser
- id, email, password_hash, name, role (SUPER_ADMIN/SUPPORT)
> Completely separate from any university's Admin — this is your own
> platform team's login, stored only in the Control-Plane DB.

### AuditLog
- id, actor_type (SUPER_ADMIN/TENANT_ADMIN/SYSTEM), actor_id, tenant_id
  (nullable — null for platform-wide actions), action, metadata (JSON),
  created_at

### Control-Plane DB Relationships
```
Tenant       (1) ──< (N) Subscription
Plan         (1) ──< (N) Subscription
Subscription (1) ──< (N) Invoice
Invoice      (1) ──< (N) Payment
Tenant       (1) ──< (N) UsageMetric
Tenant       (1) ──< (N) AuditLog
```

---

## Total Entity Count: 21
- 13 in Tenant DB (repeated per university)
- 8 in Control-Plane DB (single shared copy)

## Action Items
> Draw **two separate ER diagrams** in draw.io:
> 1. `database/tenant-db-er-diagram.png` — the 13 tenant entities
> 2. `database/control-plane-er-diagram.png` — the 8 control-plane entities
>
> Keep them as two files, not one combined diagram — they're different
> databases with no foreign keys crossing between them (only the loose
> `tenant_id` reference from Control-Plane back to "which university,"
> resolved at the application layer, not a real DB-level FK, since
> they're literally separate database instances).

## Design Notes
- **Feedback anonymity**: `Response`/`Feedback` should NOT expose
  `student_id` to Faculty-facing APIs — enforce at the service layer, not
  just hide in UI.
- Use `cycle_id` (e.g. "2026-SEM1") consistently across Feedback and
  AI_Report so reports can be generated per academic cycle.
- Add indexes on (Tenant DB): `feedback.subject_id`, `feedback.student_id`,
  `response.feedback_id`, `ai_report.subject_id`, `enrollment.student_id`,
  `enrollment.subject_id`.
- Add indexes on (Control-Plane DB): `tenant.subdomain` (unique, most
  frequently queried column — every single request hits this), `
  subscription.tenant_id`, `invoice.tenant_id`.
- **Tenant provisioning flow**: when a new university signs up —
  1. Insert row into `Tenant` (Control-Plane DB), status = PENDING
  2. Spin up new Tenant DB (automated script/service)
  3. Run Flyway migrations against the new Tenant DB
  4. Seed default `Role`/`Permission` rows in the new Tenant DB
  5. Update `Tenant.status` → ACTIVE, cache subdomain mapping in Redis
- **Plan enforcement**: before allowing an action (e.g. adding the 2001st
  student on a Middle plan capped at 2000), check `UsageMetric` against
  `Plan.feature_limits` — this check happens in the app layer using data
  from **both** databases (Control-Plane for the limit, Tenant DB for the
  current count), so cache the plan limits per tenant to avoid a
  cross-database round trip on every write.
