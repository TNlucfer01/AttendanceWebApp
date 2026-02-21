## Calendar Module

> Manages holidays, working days, and the academic calendar.
> Managed by: [[Principal]] (Admin can add emergency system-level holidays only)

---

### Calendar Data

| Field | Details |
|---|---|
| Date | Specific calendar date |
| Day Type | Working Day / Holiday / Half-Day |
| Holiday Name | e.g., "Pongal", "Republic Day", "University Exam Holiday" (null if working day) |
| Scope | College-wide (default) — no per-department holidays |
| Created By | Principal (Admin for emergency holidays only) |

---

### Rules
- The academic calendar is **college-wide** — all departments follow the same holiday schedule
- Principal sets the calendar **before the semester starts** (during UPCOMING status in [[AcadmicYear]])
- Holidays can be added **retroactively** (e.g., unexpected holiday declared by government) — but attendance already marked on that day is NOT automatically deleted; HOD must manually review
- **Saturdays** follow the Principal's working-day configuration (Mon–Fri or Mon–Sat)
- **Sundays** are always holidays — cannot be set as working days

### How Calendar Integrates with Other Modules
- [[Leave]]: Blocks leave requests for holidays ("Selected date is a holiday")
- [[TimeTable]]: All period slots for a holiday are auto-cancelled; no attendance required
- [[AcadmicYear]]: Total Working Days = Total days in semester minus Calendar holidays

---

### Edge Cases
- **EC-CAL-01**: What if a holiday is declared on a day where attendance has already been marked? → Attendance records for that day are flagged. HOD reviews and decides: exclude that day from total count, or keep it.
- **EC-CAL-02**: What if Principal forgets to add holidays before the semester starts? → Allow adding holidays anytime. System sends a warning if a holiday is added retroactively.
- **EC-CAL-03**: What if two holidays are added for the same date? → Block duplicate. Only one entry per date.
- **EC-CAL-04**: What if a half-day is declared — which periods count? → For half-days, Principal specifies which periods are cancelled (e.g., "Periods 5–8 cancelled"). Only those slots are excluded.
- **EC-CAL-05**: What if a government-declared holiday falls on a Saturday that was set as a working day? → Holiday takes precedence. Saturday slots are cancelled for that week.
