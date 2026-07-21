# Market Research — Moodle Feedback (Activity Module)

## Kya hai
Moodle LMS ka built-in "Feedback" activity module. Course-level surveys banane ke liye use hota hai — bahut open-source colleges/universities isi LMS pe chalte hain.

## Feature Findings
| Area | Finding |
|---|---|
| Data collection | Course/activity level feedback forms — rating scale + text response types |
| Analysis | Basic response summary table + downloadable Excel/analysis page |
| Sentiment analysis | ❌ Nahi. Koi NLP/AI layer core module me nahi hai |
| Roles/RBAC | Moodle ka apna role system hai (Teacher/Student/Manager), but ye course-management ke liye hai, feedback-intelligence dashboards ke liye nahi |
| Multi-tenant | Moodle multi-tenant ho sakta hai (multi-site), but feedback module khud cross-department comparison nahi karta |
| Notifications | Sirf Moodle ka generic activity-notification system, "AI insight ready" jaisa kuch nahi |
| UI/UX | Dated — feedback module ka interface last major redesign purana hai, mobile experience weak |
| Cost | Free (open-source), hosting/maintenance cost institution ko uthana padta hai |
| Extensibility | Plugins available (e.g., some third-party analytics plugins), but official AI sentiment plugin nahi |

## Kya missing hai (Gap)
1. UI/UX dated — naye users ko confusing lagta hai.
2. Sirf ek course ke andar ka feedback dikhata hai — HOD-level ya department-wide comparative view nahi.
3. Trend across semesters manually track karna padta hai (export + Excel).
4. AI insight ka koi trace nahi — pure "form + tally" system.

## EduInsight ke liye Takeaway
Moodle already colleges ke LMS stack me hai, isliye EduInsight ko compete nahi karna — **integrate/complement** karna better strategy hai (optional Moodle plugin/connector future roadmap me rakh sakte ho, jaisa Explorance Blue Moodle ke saath integrate karta hai).
