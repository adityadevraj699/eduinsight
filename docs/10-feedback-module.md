# 10 — Feedback Module (Phase 8)

## Scope
Core module — Student submits feedback for a Subject; system stores
Responses and emits an event for AI processing.

## Pre-requisites
Auth, Student, Faculty, Subject modules must exist (Phase 6–7 complete).

## Entities Involved
`Feedback`, `Response`, `Question` (see `05-database.md`)

## Flow
1. Student requests active question set: `GET /api/feedback/form?subjectId=`
2. Student submits: `POST /api/feedback` with array of `{questionId, ratingValue|textValue}`
3. Backend:
   - Validates student is enrolled in subject.
   - Validates one submission per student per subject per cycle (no dupes).
   - Saves `Feedback` + `Response` rows in a transaction.
   - Publishes Kafka event `feedback.submitted` with `feedbackId`.
   - Returns `201 Created` immediately (does NOT wait for AI).

## Checklist
- [ ] `Question`, `Feedback`, `Response` entities + migrations
- [ ] `FeedbackController` — get form, submit, check status
- [ ] Enrollment validation (Student ↔ Subject)
- [ ] Duplicate submission guard (unique constraint: student_id + subject_id + cycle_id)
- [ ] Kafka producer: publish `feedback.submitted` event
- [ ] Anonymity rule enforced: Faculty-facing DTOs never include `student_id`
- [ ] Unit + integration tests

## Design Decisions to Note
- Anonymity is enforced **at the service/DTO layer**, not just hidden in
  UI — this matters for both correctness and any legal/policy requirement
  around anonymous feedback.
- Kafka event payload should be minimal (`feedbackId`, `subjectId`,
  `cycleId`) — AI consumer fetches full data from DB, avoiding stale/large
  event payloads.

## Done Criteria
Student can submit feedback for an enrolled subject exactly once per
cycle; a Kafka event is emitted; Faculty cannot see which student
submitted which response.
