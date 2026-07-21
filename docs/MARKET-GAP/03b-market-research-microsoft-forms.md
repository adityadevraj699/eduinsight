# Market Research — Microsoft Forms

## Kya hai
Microsoft 365 ka form/survey tool, Office 365 Education licenses ke saath free milta hai. Google Forms se thoda better analytics UI deta hai.

## Feature Findings
| Area | Finding |
|---|---|
| Data collection | Structured + free text, branching logic thoda better hai Google Forms se |
| Analysis | Built-in "Insights" tab — auto charts, average score, response breakdown by choice |
| Sentiment analysis | ❌ Nahi (kuch enterprise M365 Copilot add-ons me experimental text analytics aa rahi hai, but standard Forms me nahi) |
| Roles/RBAC | ❌ Nahi — sirf co-owner sharing via M365 permissions, faculty/HOD/admin hierarchy nahi |
| Multi-tenant | Ek M365 tenant = ek institution; cross-institution scaling ka design hi nahi hai |
| Notifications | Response aane par email — koi "AI report ready" trigger nahi |
| Reports | Excel export ho sakta hai, PowerBI ke saath connect karke advanced dashboard bana sakte ho (extra setup + license cost) |
| Cost | Free with M365 Education, but PowerBI Pro extra paid |
| Integration | Teams/Outlook ke saath tight integration, LMS (Moodle/Canvas) se native nahi |

## Kya missing hai (Gap)
1. Sentiment/theme extraction se free-text comments literally unread reh jaate hain.
2. Actionable insight generate nahi hota — sirf number aur chart.
3. PowerBI jodo to advanced ho sakta hai, but wo ek alag paid+technical setup hai jo chhoti colleges nahi karti.
4. Faculty ko proactively kuch nahi milta jab tak koi manually report nikal ke na de.

## EduInsight ke liye Takeaway
MS Forms + PowerBI milakar ek "DIY" analytics ban sakta hai, but wo har college ke IT team ki capability aur budget pe depend karta hai. EduInsight isko out-of-box de sakta hai — AI summary + RBAC dashboard bina extra PowerBI license ke.
