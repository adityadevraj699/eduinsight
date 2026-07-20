# 04 — Feature List & Tech Stack

## Feature List (Modules)

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

Each of these will become its own package/module in the backend
(package-by-feature) and its own module folder in the frontend.

## Tech Stack (Final)

### Backend
| Tech | Purpose |
|---|---|
| Java 21 | Language (virtual threads help with async I/O) |
| Spring Boot | Core framework |
| Spring Security | Auth + RBAC |
| Spring AI | Wrapper to call LLM for feedback analysis |
| Flyway | Database migrations |

### Frontend
| Tech | Purpose |
|---|---|
| React | UI library |
| TypeScript | Type safety |
| Tailwind CSS | Styling |

### Database & Infra
| Tech | Purpose |
|---|---|
| PostgreSQL | Primary relational DB |
| Redis | Caching (dashboard queries, session/rate-limit) |
| Kafka | Async queue — feedback → AI processing pipeline |
| Docker | Containerization |
| AWS | Deployment (EC2/ECS + RDS + S3) |

## Why These Choices (short justification — expand in ADRs)
- **PostgreSQL over MongoDB**: data is highly relational (University →
  Department → Faculty/Subject → Feedback), foreign keys and joins matter.
- **Kafka over direct sync call**: AI analysis shouldn't block feedback
  submission response time; also allows retry/replay if AI service fails.
- **Redis**: dashboard reads are frequent and repetitive (same HOD checking
  same report) — cache aggressively, invalidate on new feedback cycle.
- **Modular Monolith over Microservices**: team size = 1 (or small), scale
  requirement doesn't justify microservices overhead yet. See
  `architecture/decisions/001-monolith-vs-microservices.md`.
