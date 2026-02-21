## Leave & OD Module

> Tracks all types of student absences and approvals.
> Initiated by: [[Student]]
> Approved by: [[Staff]] (Class Advisor) → [[HOD]] (final)

---

## Types of Leave

| Type | Description |
|---|---|
| **Informed Leave** | Student submitted a leave request and it was approved |
| **Uninformed Leave** | Student was absent with no prior request, or request was rejected |
| **OD (On Duty)** | Student was absent for an official college activity (event, competition, etc.) |

> Every leave/OD request **must have a reason** attached.

---

## Leave Limit
- Set per semester by HOD (within bounds set by Principal — see [[Principal]] for max limit)
- If a student exceeds the leave limit → HOD is notified
- OD requests do **not** count toward the leave limit
- **Retroactive Leave Cap:** Max **3 retroactive leave approvals** per semester per student. After 3, system flags "Suspicious Pattern" and requires Principal approval for further retroactive leaves.

---

## Leave Approval Flow

```
Student submits Leave Request
        ↓
Class Advisor (approve / reject)
        ↓ (if approved)
HOD (final approve / reject)
        ↓ (if HOD approves)
Attendance updated as: INFORMED LEAVE
        ↓ (if HOD rejects)
Attendance stays as: UNINFORMED LEAVE
```

---

## Leave Rejection & Escalation

```
Class Advisor rejects Leave
        ↓
Student can escalate to HOD directly
        ↓
HOD can override Class Advisor's rejection
        ↓ (if HOD approves override)
Attendance updated as: INFORMED LEAVE (with note: "Advisor Overridden")
```

### Escalation Safeguards
- Escalation to HOD is only available **after** the Class Advisor has explicitly rejected — not before a response
- **Max manual escalations per semester:** 3 per student. Beyond 3 → system blocks escalation; student must resolve with Class Advisor
- **Auto-escalations** (triggered by Class Advisor inactivity after 2 days — see EC-LEAVE-03) do **NOT** count toward the student's 3-escalation limit
- HOD always sees the Class Advisor's rejection reason before deciding
- System tracks escalation frequency per student; flags students who escalate every leave request

---

## OD Approval Flow

```
Student submits OD Request (with reason + event name)
        ↓
Class Advisor (approve / reject)
        ↓ (if approved)
HOD (final approve / reject)
        ↓ (if HOD approves)
Attendance updated as: OD (does not affect attendance %)
```

### OD Safeguards
- OD must have a **verifiable reason** — event name is mandatory; proof document is optional (experimental)
- HOD can maintain a pre-approved **OD Events List** each semester (e.g., "Tech Fest", "University Sports Day")
- **Max OD days per semester:** 10 days per student. Beyond 10 → requires Principal approval
- System tracks OD frequency per student; flags outliers (e.g., more than 5 ODs in a month)

---

### Edge Cases
- **EC-LEAVE-01**: What if a student submits a leave for a future date and then attends class? → The system auto-detects the student was marked Present by Subject Staff. Leave status becomes "Cancelled — Student Attended." Leave limit is not consumed.
- **EC-LEAVE-02**: What if a leave request is submitted for a holiday or non-working day? → Block it. System checks against the academic calendar and rejects submission with "Selected date is a holiday."
- **EC-LEAVE-03**: What if the Class Advisor does not respond to a leave request within 2 days? → Auto-escalate to HOD. Notify both parties. **This auto-escalation does NOT count** toward the student's manual escalation limit of 3.
- **EC-LEAVE-04**: What if a student submits two OD requests for the same date? → Block duplicate. Only one request per date is allowed.
- **EC-LEAVE-05**: What if the HOD approves leave after attendance for that day has already been marked as Absent? → Attendance record for that day is updated retroactively. Audit log records the change.
- **EC-LEAVE-06**: What if a student exhausts their leave limit and submits another leave? → Allow submission but flag it: "Leave limit reached. HOD will be notified." HOD decides whether to grant an exception.
- **EC-LEAVE-07**: What if an OD is approved but only for half the day (e.g., 2 periods)? → Partial OD — only the specific period slots during those hours are marked `OD`. Other periods retain their original status (`P` if present, `A` if absent, `IL`/`UL` if on leave). Each period is tracked independently.
- **EC-LEAVE-08**: What if a student's leave is rejected but automated SMS/notifications have already been sent to parents? → Notification system sends a follow-up "Leave request was rejected" message to parent and student.
- **EC-LEAVE-09**: What if a student submits a leave request for a date **before the semester start** or **after the semester end**? → Block it. System validates against the active [[AcadmicYear]] dates. Show error: "Selected date is outside the current semester."
- **EC-LEAVE-10**: What if a student has both a Leave and an OD request for the same date? → Block at data-model level. Only **one request** (Leave or OD) is allowed per student per date. Student must cancel one before submitting the other.
- **EC-LEAVE-11**: What if an approved OD event is cancelled by the college? → HOD can **batch-revoke** all ODs linked to that event. All affected students are notified. Attendance records revert: `OD` → `A` (Absent) unless the student was actually present (marked P by staff). Reverted records are audit-logged.
- **EC-LEAVE-12**: What if Principal lowers the max leave limit mid-semester and students have already exceeded the new limit? → **Already-used leaves are not clawed back.** The new limit applies going forward only. Students who already exceeded it are flagged as "Over Limit" for HOD review, but their past approvals remain valid.
