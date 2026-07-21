# Market Research — Google Forms

## What It Is
Google's free, general-purpose form builder. Most colleges use it to collect student feedback because it's free and already available within Google Workspace for Education.

## Feature Findings
| Area | Finding |
|---|---|
| Data collection | Supports both structured (MCQ, rating, linear scale) and free-text responses |
| Analysis | Only basic auto-summary charts (bar/pie) and averages — no AI/NLP |
| Sentiment analysis | ❌ None at all. Free-text comments appear as a raw list with no theme grouping |
| Roles/RBAC | ❌ None. Only "form owner" vs. "respondent" — no Faculty/HOD/Admin role-based access system exists |
| Multi-tenant | ❌ No — one institution uses one Google account/workspace; cross-department aggregation must be done manually (in Sheets) |
| Notifications | Only an email to the owner when a new response arrives — no "insight ready" loop for faculty |
| Reports | CSV/Sheets export is possible, but there's no comparative/trend reporting engine |
| Cost | Free (with Workspace for Education) |
| Integration | No LMS/SIS integration |

## What's Missing (Gap)
1. No AI/sentiment layer — only raw averages.
2. The concept of role-based dashboards (Faculty/HOD/Admin) doesn't exist at all.
3. Trend tracking across semesters/years has to be done manually in Excel.
4. Anonymity control is basic — the form owner can see everything, so legal/anonymity requirements aren't handled.

## Takeaway for EduInsight
Google Forms is a collection tool, not an intelligence tool. EduInsight's differentiation fills exactly this gap — AI sentiment analysis, role-based reports, and a notification loop, none of which Forms offers.
