# 08 — Folder Structure

## Top Level (GitHub repo)
```
EduInsight/
├── backend/
├── frontend/
├── database/
├── docker/
├── architecture/
│   └── decisions/        # ADRs
├── api/
├── docs/
├── monitoring/
├── .github/workflows/    # CI/CD
├── .gitignore
└── README.md
```

## Backend (package-by-feature, modular monolith)
```
backend/src/main/java/com/eduinsight/
├── EduInsightApplication.java
├── common/
│   ├── config/          # SecurityConfig, RedisConfig, KafkaConfig
│   ├── exception/        # GlobalExceptionHandler
│   ├── util/
│   └── constants/
├── auth/
│   ├── controller/
│   ├── service/
│   ├── repository/
│   ├── entity/
│   ├── dto/
│   └── security/         # JWT filter, provider
├── university/
├── department/
├── faculty/
├── student/
├── subject/
├── feedback/
├── ai/
│   ├── controller/
│   ├── service/
│   └── client/            # Spring AI wrapper
├── report/
├── dashboard/
└── notification/
```

## Frontend (feature-based)
```
frontend/src/
├── modules/
│   ├── auth/
│   ├── feedback/
│   ├── dashboard/
│   └── faculty/
├── components/            # shared UI
├── hooks/
├── services/               # API layer (axios instances)
├── store/                  # state management
└── App.tsx
```

## Rule of Thumb
- Every module owns its full vertical slice (controller → service →
  repository → entity → dto) — never split by technical layer at the top
  level.
- `common/` only holds things genuinely shared across 3+ modules. If only
  2 modules need something, keep it duplicated until a real pattern
  emerges (avoid premature abstraction).
- When a module is ready to become its own microservice later (e.g. `ai`),
  it should be extractable by copying its folder — this is the whole point
  of the modular monolith approach.
