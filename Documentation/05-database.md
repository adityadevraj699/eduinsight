# 05 — Database Design

## Entities
```
University
Department
Faculty
Student
Subject
Course
Feedback
Question
Response
AI_Report
Notification
Role
Permission
```

## Entity Descriptions & Key Fields

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

## Relationships (high level)
```
University (1) ──< (N) Department
Department (1) ──< (N) Faculty
Department (1) ──< (N) Student
Department (1) ──< (N) Subject
Faculty    (1) ──< (N) Subject       [subject taught by faculty]
Student    (N) ──< (N) Subject       [enrollment - junction table]
Student    (1) ──< (N) Feedback
Subject    (1) ──< (N) Feedback
Feedback   (1) ──< (N) Response
Question   (1) ──< (N) Response
Subject    (1) ──< (N) AI_Report
Role       (1) ──< (N) Permission
User       (N) ──> (1) Role
```

## Action Item
> Draw the full ER Diagram in **draw.io**, export as PNG, save at
> `database/er-diagram.png`. Use this doc as the source of truth for field
> names while drawing it — keep both in sync.

## Design Notes
- Feedback anonymity: `Response`/`Feedback` should NOT expose `student_id`
  to Faculty-facing APIs — enforce at the service layer, not just hide in
  UI.
- Use `cycle_id` (e.g. "2026-SEM1") consistently across Feedback and
  AI_Report so reports can be generated per academic cycle.
- Add indexes on: `feedback.subject_id`, `feedback.student_id`,
  `response.feedback_id`, `ai_report.subject_id`.
