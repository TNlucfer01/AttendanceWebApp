## Academic Year Module

> Defines the semester lifecycle for the whole college.
> Created by: [[Principal]] — managed by [[HOD]] within their department.

---

### Academic Year Data

| Field | Details |
|---|---|
| Academic Year | e.g., "2024–2025" |
| Semester Number | Student-specific based on their year (1–8). Not department-level. |
| Semester Start Date | e.g., 01-Jan-2025 (**college-wide**, set by Principal) |
| Semester End Date | e.g., 30-May-2025 (**college-wide**, set by Principal) |
| Status | Upcoming / Active / Ended / Archived |
| Semester Length (weeks) | Set by Principal |
| Total Working Days | Computed from [[Calendar]] minus holidays |

> ⚠️ **Academic year and semester dates are college-wide** (all departments follow the same calendar, as per Tamil Nadu engineering college norms). The semester number (1–8) is student-specific based on their year of study.

---

### Semester Lifecycle

```
Principal creates Academic Year + Semester
        ↓
Status: UPCOMING
        ↓ (on start date or manually activated)
Status: ACTIVE
  → Timetables go live
  → Attendance marking begins
        ↓ (Principal triggers semester-end)
Status: ENDED
  → New attendance marking is blocked
  → HOD reviews and approves final records
        ↓ (HOD/Principal confirms archive)
Status: ARCHIVED
  → All attendance records are frozen (read-only)
  → Students are promoted to the next semester/year
```

---

### Student Promotion Rules
- Students move from Semester N → Semester N+1 upon archive
- Students in Sem 8 (final semester) are marked as **Graduated**
- Students who fail (if result integration is added later) are flagged — promotion is blocked for them

---

### Edge Cases
- **EC-AY-01**: What if Principal tries to start a new semester while the previous one is still Active? → Block it. Only one semester can be Active per department at a time.
- **EC-AY-02**: What if the semester end date passes but Principal hasn't manually ended it? → System auto-flags it as "Overdue — Semester not closed." Sends reminder to Principal and each HOD.
- **EC-AY-03**: What if a student has unresolved pending leave requests at semester end? → Semester cannot be archived until all pending requests are resolved (approved/rejected). HOD is notified.
- **EC-AY-04**: What if the Principal changes semester start/end dates after it is already Active? → Allow date change with a confirmation warning. All affected timetables display the updated dates.
- **EC-AY-05**: What if a student was added to a class after the semester started? → Their attendance is tracked from their date of joining. Attendance % is calculated only from their join date onwards, not from semester start.
- **EC-AY-06**: What if promotion is triggered but a student has 0 classes attended? → Flag the student for manual review by HOD before promotion is confirmed.
- **EC-AY-07**: What if a new Academic Year is created with dates that overlap the current active year? → Block it. System validates date ranges — new year's start date must be after the current year's end date. Show error: "Date range overlaps with [existing year]."