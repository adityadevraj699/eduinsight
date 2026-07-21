# Market Research — Google Forms

## Kya hai
Google ka free, general-purpose form builder. Zyadatar colleges isi se student feedback collect karte hain kyunki free hai aur Google Workspace for Education me already available hai.

## Feature Findings
| Area | Finding |
|---|---|
| Data collection | Structured (MCQ, rating, linear scale) + free-text — dono support karta hai |
| Analysis | Sirf basic auto-summary charts (bar/pie) aur average — koi AI/NLP nahi |
| Sentiment analysis | ❌ Bilkul nahi. Free-text comments sirf raw list ki tarah dikhte hain, koi theme grouping nahi |
| Roles/RBAC | ❌ Nahi. Sirf "form owner" vs "responder" — Faculty/HOD/Admin jaisa role-based access system hi nahi hai |
| Multi-tenant | ❌ Nahi — ek institution ke liye ek Google account/workspace, cross-department aggregation manual (Sheets me karna padta hai) |
| Notifications | Sirf naya response aane par owner ko email — faculty ko "insight ready hai" wala loop nahi |
| Reports | CSV/Sheets export possible, but koi comparative/trend report engine nahi |
| Cost | Free (Workspace for Education ke saath) |
| Integration | Koi LMS/SIS integration nahi |

## Kya missing hai (Gap)
1. Koi AI/sentiment layer nahi — sirf raw averages.
2. Role-based dashboards (Faculty/HOD/Admin) ka concept hi exist nahi karta.
3. Trend tracking across semesters/years manually Excel me karna padta hai.
4. Anonymity control basic hai (form owner sab dekh sakta hai — legal/anonymity requirement handle nahi hota).

## EduInsight ke liye Takeaway
Google Forms ek collection tool hai, intelligence tool nahi. EduInsight ka differentiation exactly yahi gap fill karta hai — AI sentiment + role-based reports + notification loop, jo Forms me bilkul nahi hai.
