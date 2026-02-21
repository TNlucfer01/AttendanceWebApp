## HOD Module

> The HOD (Head of Department) manages everything inside their department.
> Created by: [[Principal]]

### Department Management
1. Manage classes within their department (CRUD)
2. Create [[classRoom]] and assign a Class Advisor — **must be approved by Principal**
3. Add and manage [[Subject]] for each class/semester in their dept

### Timetable
4. Create and manage [[TimeTable]] for each class in their department
5. Override a specific period for a special class (ad-hoc change)
6. Enable Saturday timetable if allowed by Principal

### Attendance Management
7. View attendance of **any individual student** in their department
8. View department-wide attendance summary (by class, by subject, by date)
9. Approve or reject **attendance change requests** raised by students (forwarded by Class Advisor)

### Audit Trail for Attendance Changes
> Every attendance modification MUST be logged with:
- Who requested the change
- Who approved it
- Old attendance value → New attendance value
- Timestamp of the change
- These logs are visible to **Principal and Admin only** — HOD can see their own approvals but cannot edit the log

### Self-Approval Block
- If HOD is also a Subject Staff for a class, they **cannot approve their own attendance corrections** for that class
- If HOD submits a correction for a class where they are the Subject Staff → request is escalated to **Principal** instead
- If HOD is also the **Class Advisor** for a class, they **cannot be both first-level and final approver** for the same leave/OD request. In this case, the HOD acts as Class Advisor (first approval), and the request is auto-escalated to **Principal** for final approval instead of HOD self-approving

### Leave & OD
10. Final approver for **Leave** and **OD** requests (after Staff forwards them)
11. Set per-semester **leave limit** for their department (within Principal's bounds)
    - **Validation:** HOD's leave limit must be ≥ 1 and ≤ Principal's system-wide max leave limit (see [[Principal]])
    - If HOD tries to set a limit above Principal's max → blocked with error: "Exceeds system-wide limit of [X] days"

### Reports
12. Generate and export attendance reports for their department (PDF/Excel)

---

### HOD Leave / Absence

> HOD leave is **not formally tracked** in this system (same as Staff). However, the system must handle the HOD's absence gracefully.

#### When HOD is Absent

| Situation | What Happens |
|---|---|
| **Leave/OD approvals pending** | Auto-escalate to **Principal** after 2 days of inaction (similar to Class Advisor auto-escalation). Principal can approve/reject on HOD's behalf. |
| **Timetable changes needed** | Blocked until HOD returns. Only emergency overrides (e.g., staff absent substitution) can be handled by Principal. |
| **Attendance corrections pending** | Escalated to Principal if unresolved for 3+ days. |
| **Class creation / subject management** | Paused until HOD returns. No delegation — these are HOD-specific actions. |

#### Acting HOD
- Principal can assign a **senior staff member** as **Acting HOD** for a defined period
- Acting HOD gets the same permissions as HOD for that department during the period
- All actions by Acting HOD are audit-logged with an "Acting HOD" tag
- When original HOD returns, Acting HOD access is **automatically revoked**

---

### Edge Cases
- **EC-HOD-01**: What if the HOD creates a class but Principal is unavailable to approve? → Class stays in "Pending Approval" state. HOD cannot assign students or timetable until approved.
- **EC-HOD-02**: What if the HOD assigns a Subject Staff to a class outside their department? → Allow it, as some subjects (e.g., Maths, English) are cross-department. Log it clearly.
- **EC-HOD-03**: What if the HOD tries to delete a class that has active attendance records? → Block deletion. Must archive the class and end the semester first.
- **EC-HOD-04**: What if an HOD approves a leave request but the leave window has already passed (e.g., student requested leave for yesterday)? → System flags it as a "Retroactive Leave Approval." Attendance is updated but audit-logged.
- **EC-HOD-05**: What if the timetable has a conflict (same staff, same time, two classes)? → System auto-detects the conflict and blocks saving. Shows which classes/periods conflict.
- **EC-HOD-06**: What if the HOD is changed mid-semester? → Old HOD's decisions (timetable, approvals) remain. New HOD takes over from that point. No data is lost.
- **EC-HOD-07**: What if the HOD sets a leave limit of 0? → Minimum leave limit must be 1 per semester. System enforces this with validation.
- **EC-HOD-08**: What if two staff members simultaneously submit an attendance correction for the same student/period? → System uses **optimistic locking** — first submission wins. Second submitter sees: "This record was already modified. Please refresh and retry."
- **EC-HOD-09**: What if the HOD is absent and there's no Acting HOD assigned? → Pending approvals auto-escalate to Principal after 2 days. Principal is shown a warning: "HOD for [Dept] is absent. Consider assigning an Acting HOD."
- **EC-HOD-10**: What if an Acting HOD makes a decision the returning HOD disagrees with? → HOD cannot reverse Acting HOD's decisions unilaterally. Must raise it to Principal. All Acting HOD decisions are audit-logged for review.