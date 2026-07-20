# 06 — API Design

> No coding yet. This is the contract — write it in Swagger/OpenAPI style
> before touching a controller. Full spec will live in `api/openapi.yaml`.

## Auth
```
POST   /api/auth/register        - Register user (Admin only, or student self-signup)
POST   /api/auth/login           - Login → returns JWT
POST   /api/auth/refresh         - Refresh access token
POST   /api/auth/forgot-password - Trigger password reset email
POST   /api/auth/reset-password  - Reset password with token
```

## University / Department (Admin)
```
POST   /api/universities
GET    /api/universities/{id}
POST   /api/departments
GET    /api/departments?universityId=
```

## Faculty
```
POST   /api/faculty                 - Create faculty (Admin)
GET    /api/faculty/{id}
GET    /api/faculty?departmentId=
```

## Student
```
POST   /api/students                - Create student (Admin)
POST   /api/students/bulk-import    - CSV bulk import
GET    /api/students/{id}
GET    /api/students?departmentId=
```

## Subject
```
POST   /api/subjects
GET    /api/subjects?departmentId=&semester=
POST   /api/subjects/{id}/enroll    - Enroll student in subject
```

## Feedback
```
GET    /api/feedback/form?subjectId=      - Get active question set for a subject
POST   /api/feedback                      - Submit feedback (Student)
GET    /api/feedback/status?subjectId=    - Has current student submitted?
```

## AI
```
POST   /api/ai/analyze/{feedbackId}   - Trigger analysis (internal/Kafka consumer, not public)
GET    /api/ai/report?subjectId=&cycleId=   - Get AI report for a subject/cycle
```

## Dashboard
```
GET    /api/dashboard/faculty            - Faculty's own subject-wise summary
GET    /api/dashboard/hod                - Department-wide comparison
GET    /api/dashboard/admin              - University-wide analytics
```

## Reports
```
GET    /api/reports/subject/{id}         - Detailed report for one subject
GET    /api/reports/export?subjectId=&format=pdf   - Export report
```

## Notifications
```
GET    /api/notifications                - List current user's notifications
PATCH  /api/notifications/{id}/read      - Mark as read
```

## Conventions
- All endpoints prefixed `/api`, versionless for now (`/api/v1` if you plan
  public API consumers later).
- JWT in `Authorization: Bearer <token>` header.
- Standard error shape:
```json
{
  "timestamp": "...",
  "status": 400,
  "error": "BAD_REQUEST",
  "message": "human readable message",
  "path": "/api/feedback"
}
```
- Pagination on list endpoints: `?page=0&size=20&sort=createdAt,desc`

## Next Step
Convert this into `api/openapi.yaml` (formal OpenAPI 3.0 spec) once field
names are finalized from the Database Design doc — request/response DTOs
should mirror entity fields but hide internal-only fields (e.g. never
expose `student_id` in faculty-facing feedback responses).
