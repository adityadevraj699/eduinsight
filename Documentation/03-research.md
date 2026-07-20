# 03 — Market / Competitor Research

## Purpose
Before designing EduInsight, we study existing tools to find the gap our
product fills. This document is meant to be filled in *by you* as you
research — treat the questions below as your research checklist.

## Tools to Study

| Tool | Study Link/Notes | Your Findings |
|---|---|---|
| Google Forms | forms.google.com | Fill in after research |
| Microsoft Forms | forms.office.com | |
| Moodle Feedback module | moodle.org | |
| Canvas LMS | instructure.com | |
| AI Sentiment Analysis basics | Read: what is sentiment analysis, VADER vs LLM-based | |

## Questions to Answer for Each Tool
1. Who is it built for (teacher, student, admin)?
2. How is feedback collected (form, rating scale, free text)?
3. What happens to the data **after** collection? Is there any analysis?
4. Does it support roles/permissions (student vs faculty vs admin views)?
5. Is there any AI/NLP layer, or is it just charts of averages?
6. What would a Faculty member complain is missing if they used this tool
   for a semester?

## Synthesis (fill this after research)
> Write 4–5 sentences: "Across all these tools, the common gap is ___.
> EduInsight differentiates by ___."

## Notes on AI Sentiment Analysis (starter reading)
- **Sentiment Analysis**: classifying text as positive/negative/neutral.
- Two broad approaches:
  - **Lexicon-based** (e.g. VADER) — fast, no training needed, less
    accurate on sarcasm/context.
  - **LLM-based** (e.g. calling an LLM with a structured prompt) — more
    accurate, understands context, costs more per call.
- For EduInsight, plan is: **Spring AI + LLM-based analysis**, batched via
  Kafka consumer to control cost and avoid blocking the request thread.

---
*Fill in the "Your Findings" column as you actually research each tool —
this becomes your justification when someone in an interview asks "why did
you build this instead of using Google Forms?"*
