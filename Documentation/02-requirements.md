# 02 — Requirement Analysis

## Actors

| Actor | Description |
|---|---|
| **Student** | Submits feedback for subjects/faculty they are enrolled with |
| **Faculty** | Views AI-analyzed feedback & reports for their own subjects |
| **HOD** | Views department-wide reports, compares faculty/subjects |
| **Admin** | Manages university, departments, faculty, students, subjects; sees platform-wide reports |

## Functional Requirements

### 1. Authentication
- FR1.1 — User can register (Admin creates Faculty/Student accounts; self
  signup optional for students with university email domain validation).
- FR1.2 — User can log in via email + password → JWT issued.
- FR1.3 — Role-Based Access Control (RBAC): Student / Faculty / HOD / Admin.
- FR1.4 — Password reset flow.

### 2. University / Department Management (Admin)
- FR2.1 — Admin can create/update/deactivate a University.
- FR2.2 — Admin can create/update Departments under a University.

### 3. Faculty Management
- FR3.1 — Admin/HOD can add Faculty, assign to Department.
- FR3.2 — Faculty can be assigned to one or more Subjects.

### 4. Student Management
- FR4.1 — Admin can bulk-import Students (CSV) or add individually.
- FR4.2 — Student is linked to a Department + enrolled Subjects.

### 5. Subject / Course Management
- FR5.1 — Admin/HOD can create Subjects under a Department.
- FR5.2 — Subject linked to Faculty + enrolled Students + Semester.

### 6. Feedback
- FR6.1 — Student submits feedback per Subject (rating questions +
  free-text comments).
- FR6.2 — Feedback form is configurable (Admin can define Question sets).
- FR6.3 — Feedback is anonymous to Faculty (visible identity only to Admin,
  if legally required).
- FR6.4 — One feedback submission per Student per Subject per feedback
  cycle (no duplicate spam).

### 7. AI Analysis
- FR7.1 — On feedback submission (or batch), AI service analyzes free-text
  responses for **sentiment** (positive/neutral/negative).
- FR7.2 — AI extracts **key themes/topics** (e.g. "pacing", "clarity",
  "support material").
- FR7.3 — AI generates a **natural-language summary** per subject per
  cycle.
- FR7.4 — Async processing via Kafka — feedback submission should not wait
  on AI response.

### 8. Reports & Dashboard
- FR8.1 — Faculty Dashboard: subject-wise sentiment trend + AI summary.
- FR8.2 — HOD Dashboard: department-wide comparison across faculty/subjects.
- FR8.3 — Admin Dashboard: university-wide analytics.
- FR8.4 — Exportable report (PDF/CSV).

### 9. Notifications
- FR9.1 — Email notification when AI report is ready.
- FR9.2 — In-app notification for new feedback cycle open (to students).

## Non-Functional Requirements

| Category | Requirement |
|---|---|
| Security | JWT auth, RBAC enforced at API layer, feedback anonymity |
| Performance | Dashboard queries < 500ms (cached via Redis) |
| Scalability | Async AI processing via Kafka so it doesn't block core flow |
| Availability | Core feedback submission must work even if AI service is down (graceful degradation) |
| Maintainability | Modular monolith — package-by-feature, documented ADRs |
| Observability | Centralized logging + basic metrics (Prometheus/Grafana) |

## Out of Scope (V1)
- Multi-language AI analysis (English only for V1)
- Native mobile app (web-responsive only)
- Payment/billing (not a commercial SaaS in V1)
