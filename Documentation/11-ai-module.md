# 11 — AI Module (Phase 9)

## Scope
Consumes `feedback.submitted` events, runs sentiment + theme analysis via
Spring AI (LLM), stores `AI_Report`, publishes `report.ready` event.

## Flow
```
Kafka consumer (feedback.submitted)
      │
      ▼
Fetch Feedback + Responses from DB
      │
      ▼
Build prompt with free-text responses for that Subject/cycle
      │
      ▼
Call LLM via Spring AI
      │
      ▼
Parse structured output: { sentimentLabel, sentimentScore, keyThemes[], summaryText }
      │
      ▼
Save AI_Report → PostgreSQL
      │
      ▼
Publish "report.ready" event → Notification module picks it up
```

## Prompt Design (starter idea — refine while building)
> "You are analyzing anonymous student feedback for a college subject.
> Given the following free-text comments, return JSON with: overall
> sentiment (positive/neutral/negative), a sentiment score (-1 to 1), 3-5
> key themes, and a 2-3 sentence actionable summary for the instructor."

Force **structured JSON output** from the LLM (Spring AI supports response
format / output parsers) so you don't have to regex-parse free text.

## Checklist
- [ ] `AI_Report` entity + migration
- [ ] Kafka consumer for `feedback.submitted`
- [ ] Batching strategy: analyze per-cycle (wait for cycle to close) or
      per-submission? → **Recommendation: batch per cycle-close** for
      subject-level themes to be meaningful (analyzing 1 comment at a time
      gives noisy, low-value themes).
- [ ] Spring AI client wrapper (`ai/client/`)
- [ ] Structured output parsing + validation
- [ ] Error handling: LLM call failure → retry with backoff, dead-letter
      queue after N retries
- [ ] Publish `report.ready` Kafka event
- [ ] Unit tests with mocked LLM responses

## Design Decisions to Note
- **Batch vs per-submission analysis**: batching per cycle-close is
  usually better for actionable, non-noisy themes — document this choice
  and the trade-off (less "real-time" but more meaningful) in an ADR.
- Cost control: cap max tokens per prompt, truncate/sample if a subject
  has hundreds of free-text responses.

## Done Criteria
When a feedback cycle closes for a subject, an `AI_Report` row exists with
sentiment, themes, and summary — visible via `GET /api/ai/report`.
