# Market & Competitor Research — EduInsight

## Purpose

Before designing EduInsight, we studied existing tools to identify the gap our product fills. This document combines the original research checklist with completed findings, expanded to cover the wider competitive landscape — not just basic form tools, but also AI-native course-evaluation platforms and India-specific college ERP/accreditation systems. Leaving these out would make a "no one does AI analysis" claim inaccurate.

---

## Research Questions

For each tool, we answer:

1. Who is it built for (teacher, student, admin)?
2. How is feedback collected (form, rating scale, free text)?
3. What happens to the data **after** collection? Is there any analysis?
4. Does it support roles/permissions (student vs. faculty vs. admin views)?
5. Is there any AI/NLP layer, or is it just charts of averages?
6. What would a Faculty member complain is missing after a semester of use?

---

## Tools Studied

| Tool | Category | Documentation Link | Source File | Key Findings |
|---|---|---|---|---|
| Google Forms | Basic form tool | https://forms.google.com | [03a-market-research-google-forms.md](MARKET-GAP/03a-market-research-google-forms.md) | Free; collects structured + free-text responses. No AI, no roles, no trend tracking. Only basic auto-generated charts; free-text sits as an unsorted raw list. |
| Microsoft Forms | Basic form tool | https://forms.office.com | [03b-market-research-microsoft-forms.md](MARKET-GAP/03b-market-research-microsoft-forms.md) | Free with Microsoft 365 Education. Better built-in "Insights" tab than Google Forms; can pair with Power BI for real dashboards, but that requires an extra paid license and IT setup. No AI, no institutional roles. |
| Moodle Feedback module | Basic form tool (LMS-integrated) | https://moodle.org | [03c-market-research-moodle-feedback.md](MARKET-GAP/03c-market-research-moodle-feedback.md) | Built into the LMS many colleges already run. Course-level only; dated UI; no AI layer; no HOD/Admin-wide comparison view. |
| Canvas LMS | Basic form tool (LMS-integrated) | https://www.instructure.com/canvas | [03d-market-research-canvas-lms.md](MARKET-GAP/03d-market-research-canvas-lms.md) | Feedback is a minor feature of a full enterprise LMS. AI analysis only available via costly third-party LTI add-ons (Qualtrics, Explorance). |
| Explorance Blue | AI-native course-eval SaaS | https://www.explorance.com/blue | [03e-market-research-ai-native-competitors.md](MARKET-GAP/03e-market-research-ai-native-competitors.md) | AI sentiment and theme extraction with natural-language summaries. Enterprise-priced; built for Western universities; no NAAC/NBA awareness. |
| Watermark | AI-native course-eval SaaS | https://www.watermarkinsights.com | [03e-market-research-ai-native-competitors.md](MARKET-GAP/03e-market-research-ai-native-competitors.md) | Same strengths and weaknesses as Explorance — strong AI layer, no India-accreditation context. |
| Qualtrics | AI-native course-eval SaaS | https://www.qualtrics.com | [03e-market-research-ai-native-competitors.md](MARKET-GAP/03e-market-research-ai-native-competitors.md) | Enterprise survey/experience platform with AI text analysis; expensive and not India-specific. |
| Perspective AI | AI-native course-eval SaaS | https://perspective.co | [03e-market-research-ai-native-competitors.md](MARKET-GAP/03e-market-research-ai-native-competitors.md) | Uses a conversational follow-up model to improve response rates; strong AI depth, no India/NAAC-NBA fit. |
| Camu | India college ERP/accreditation | https://www.camuerp.com | [03f-market-research-india-erp-players.md](MARKET-GAP/03f-market-research-india-erp-players.md) | NAAC/NBA-compliant structured feedback tied into accreditation reporting (SAR/AQAR). No AI/sentiment layer. |
| EduSys | India college ERP/accreditation | https://www.edusys.co | [03f-market-research-india-erp-players.md](MARKET-GAP/03f-market-research-india-erp-players.md) | Same category strengths/weaknesses as Camu — compliance-first, zero AI. |
| LearnQoch | India college ERP/accreditation | https://www.learnqoch.com | [03f-market-research-india-erp-players.md](MARKET-GAP/03f-market-research-india-erp-players.md) | Accreditation-focused feedback collection; no natural-language analysis. |

---

## Detailed Findings Per Tool (Tier 1 — Basic Form Tools)

### 1. Google Forms
- **Built for:** Anyone collecting responses; no concept of Faculty/HOD/Admin roles.
- **Collection method:** Rating scale (linear scale/MCQ) and free text, both supported.
- **After collection:** Only basic auto-generated bar/pie charts and averages in the built-in "Summary" view. No theme grouping of comments — free text just sits as a raw list.
- **Roles/permissions:** None. Only "form owner" vs. "respondent" — no institutional hierarchy.
- **AI/NLP layer:** None.
- **Faculty complaint after a semester:** "I get a 4.2/5 average and 60 unread comments. I have no idea what students actually mean by 'the labs were confusing.'"

### 2. Microsoft Forms
- **Built for:** Same general-purpose audience; ships free with Microsoft 365 Education.
- **Collection method:** Structured and free-text responses, with slightly better branching logic than Google Forms.
- **After collection:** Built-in "Insights" tab (per-question breakdown), exportable to Excel; can connect to Power BI for real dashboards — but that requires a separate paid license and technical setup most colleges won't invest in just for feedback.
- **Roles/permissions:** None beyond Microsoft 365 file-sharing permissions (co-owner). No Faculty/HOD/Admin hierarchy.
- **AI/NLP layer:** None in standard Forms.
- **Faculty complaint after a semester:** "Nice charts, but nobody told me an AI report existed — because there isn't one. I only see anything if the department head personally forwards an Excel file."

### 3. Moodle Feedback Module
- **Built for:** Teachers running course-level surveys within the LMS most colleges already use.
- **Collection method:** Rating scale and text-response types inside a Moodle "activity."
- **After collection:** A response summary table plus a downloadable analysis export. No sentiment or theme extraction in the core module.
- **Roles/permissions:** Moodle's own Teacher/Student/Manager roles exist, but they govern course management, not feedback-intelligence dashboards — there's no department-wide HOD comparison view.
- **AI/NLP layer:** None in the official module; UI/UX is dated.
- **Faculty complaint after a semester:** "This only shows me my own course. I have no way to see if my subject is trending worse than last semester, or how I compare to other faculty in my department."

### 4. Canvas LMS
- **Built for:** Whole-institution LMS (courses, grading, assignments) — feedback is a minor corner feature, not the core product.
- **Collection method:** Quizzes/Surveys tool with rating and text responses, plus basic question-level statistics.
- **After collection:** No native AI analysis. Institutions that want AI sentiment must pay for a third-party LTI integration (Qualtrics or Explorance Blue), adding significant cost.
- **Roles/permissions:** LMS-wide Admin/Teacher/Student roles, not feedback-specific.
- **AI/NLP layer:** None natively; enterprise pricing makes it a non-starter for most mid-size Indian colleges as a dedicated feedback tool.
- **Faculty complaint after a semester:** "My college pays a fortune for Canvas and I still just get a spreadsheet of numbers — the 'AI insight' add-on wasn't in our contract."

---

## Answers to the 6 Research Questions (Across Tier 1 Tools)

1. **Who is it built for?** All four are built for generic form creation or course delivery — none has a concept of Faculty, HOD, and Admin as distinct dashboard consumers with different views.
2. **How is feedback collected?** All support both rating scales and free text — collection itself is a solved problem industry-wide.
3. **What happens after collection?** Google/MS Forms: basic charts only. Moodle: tabulated summary. Canvas: basic quiz statistics. None of the four do any language understanding of the comments — the free-text field is where all the useful information dies.
4. **Roles/permissions?** Only Moodle and Canvas have any role system at all, and it exists for LMS administration, not feedback intelligence — none give a HOD a "compare all faculty in my department" view.
5. **AI/NLP layer?** None of the four have one natively (Canvas can bolt one on via expensive third-party add-ons).
6. **What would Faculty complain about after a semester?** Across all four: *"I got a number, not an explanation. I don't know what to change, and I only saw the data because someone manually sent it to me."*

---

## Synthesis

Across Google Forms, Microsoft Forms, Moodle Feedback, and Canvas LMS, the common gap is that all four stop at data collection — they hand back raw averages and unread free-text comments, with no role-based reporting and no mechanism that tells a faculty member what to actually change. EduInsight differentiates by adding an AI sentiment-and-theme layer on top of collection, generating natural-language, per-subject summaries for Faculty, comparative department-wide views for HODs, and a notification loop so insights reach people automatically instead of sitting unread in a spreadsheet.

**However**, this synthesis holds only against these four basic tools. It does **not** hold against the wider global market, which already has AI-native competitors. The real, defensible gap is narrower and India-specific — see the Market Gap Analysis below.

---

## Notes on AI Sentiment Analysis

- **Sentiment analysis** classifies text as positive, negative, or neutral — and often extracts themes/topics on top.
- Two broad approaches:
  - **Lexicon-based** (e.g., VADER): fast, requires no training data, but weaker on sarcasm and context. (For example, "the labs were 'great,' if you enjoy staying till midnight" would likely be misread as positive.)
  - **LLM-based** (structured-prompt calls to an LLM): much better at context and implied meaning, but costs more per call and is slower.
- **EduInsight's plan is sound:** Spring AI with LLM-based analysis, batched asynchronously through a Kafka consumer. This avoids blocking the feedback-submission request and keeps cost predictable by batching rather than calling the LLM per individual comment.

---

## Market Gap Analysis

The market actually splits into **three tiers**, not one:

| Tier | Examples | What They Do | Weakness |
|---|---|---|---|
| **Tier 1 — Basic form tools** | Google Forms, MS Forms, Moodle Feedback, Canvas Surveys | Collection only, zero AI | Easy to beat on features, but colleges use them because they're free or already installed |
| **Tier 2 — Global AI course-eval SaaS** | Explorance Blue, Watermark, Qualtrics, Perspective AI, Student Voice AI | Already do AI sentiment, theme extraction, and natural-language summaries | Enterprise-priced, built for Western universities, zero India/NAAC-NBA awareness |
| **Tier 3 — India college ERP/accreditation suites** | Camu, EduSys, LearnQoch, Kalvisalai, Edhitch | NAAC/NBA-compliant structured feedback collection, tied into accreditation report generation (SAR/AQAR) | Feedback here is compliance paperwork — no AI/sentiment layer at all |

**The real, defensible gap:** no single product currently combines Tier 2's AI depth with Tier 3's India-accreditation context at an affordable price point. Global AI tools ignore NAAC/NBA entirely; Indian ERP tools ignore AI entirely.

### Recommended Positioning Statement for EduInsight

> "AI-powered feedback intelligence, purpose-built for Indian colleges — combining actionable sentiment/theme insights (the kind Explorance and Watermark already provide globally) with NAAC/NBA-ready structured evidence and affordable, per-institution pricing (the kind Indian ERP players provide, but without any AI)."

---

## Recommended Follow-Up Actions

1. Update the competitor table in `01-problem-statement.md` to include Tier 2 and Tier 3 — the current "no one does AI" framing is inaccurate as of 2026.
2. Add a NAAC/NBA-compatible evidence-export feature (SAR/AQAR format) to the roadmap — this is what would make EduInsight credible to Indian college procurement teams without having to build a full ERP.
3. Consider response-rate improvement mechanisms (reminders, or a conversational follow-up model like Perspective AI uses) — collection plus analysis alone doesn't fix the well-documented sub-30% response-rate problem in course evaluations.
4. Decide on a pricing model early (per-student/year, per-institution/year, or a free tier for small colleges), since "Out of Scope V1: Payment/billing" currently leaves this undecided.