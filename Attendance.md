## Attendance Record Module

> Defines the structure of a single attendance record — the core data unit of the system.
> Created by: [[Staff]] (Subject Staff) during attendance marking

---

### Attendance Record Fields

| Field | Details |
|---|---|
| Record ID | Auto-generated unique ID |
| Student | Linked to [[Student]] |
| Subject | Linked to [[Subject]] |
| Class | Linked to [[classRoom]] |
| Date | The date of the period |
| Period Number | Which period (1, 2, 3... as per [[TimeTable]]) |
| Marked By | Linked to [[Staff]] who marked it (or "System" for auto-marks) |
| Status | See status enum below |
| Timestamp | When the record was created/last modified |
| Modified By | Null if original; Staff/HOD ID if corrected |
| Modification Reason | Null if original; required if corrected |

---

### Attendance Status Enum

| Status | Code | Counts Toward Total? | Counts as Present? |
|---|---|---|---|
| **Present** | `P` | ✅ Yes | ✅ Yes |
| **Absent** | `A` | ✅ Yes | ❌ No |
| **Informed Leave** | `IL` | ✅ Yes | ❌ No (but tracked separately) |
| **Uninformed Leave** | `UL` | ✅ Yes | ❌ No |
| **On Duty (OD)** | `OD` | ❌ No (excluded) | N/A |
| **Staff Absent** | `SA` | ⏳ Temporary (see SA Lifecycle below) | Resolved to P or A by Class Advisor |
| **Suspended** | `SU` | ❌ No (excluded) | N/A |
| **Attendance Not Marked** | `NM` | ❌ No (excluded) | N/A |
| **Holiday** | `H` | ❌ No (excluded) | N/A |

---

### Attendance Percentage Calculation

```
Attendance % = (Total Present) / (Total Countable Periods) × 100

Where:
  Total Present = count of records with status = P
  Total Countable Periods = count of records with status IN (P, A, IL, UL)
  
  Excluded from count: OD, SU, NM, H
  
  Note: SA is a temporary status — it resolves to P or A (see SA Lifecycle).
  Records with status SA at calculation time are treated as NM (excluded).
```

> **Informed Leave (IL) is NOT counted as present** — it reduces attendance % but is tracked separately for reporting. The student was absent; it was just approved.

---

### SA (Staff Absent) Status Lifecycle

> `SA` is a **temporary status**, not a final state. It exists only while the Class Advisor has not yet marked attendance for the period.

```
Subject Staff is marked absent by HOD
        ↓
All period slots for that staff are set to SA (temporary)
        ↓
Class Advisor (or Substitute) marks attendance
        ↓ (student present)
Status changes: SA → P
        ↓ (student absent)
Status changes: SA → A (or IL/UL based on leave status)
        ↓ (if nobody marks within the window)
Status changes: SA → NM (Attendance Not Marked)
```

> ⚠️ At any given time, records with `SA` status are **unresolved** and excluded from attendance % until the Class Advisor resolves them.

---

### Attendance is Tracked PER-PERIOD, Not Per-Day
- Each period of each day has its own attendance record per student
- A student can be Present in Period 1 and Absent in Period 3 on the same day
- Reports can aggregate to daily, weekly, monthly, or semester views

---

### Edge Cases
- **EC-ATT-01**: What if a student has no attendance records for a given day? → They were either enrolled after the semester started or not yet assigned to the class. Their attendance % only counts from their join date (see [[AcadmicYear]] EC-AY-05).
- **EC-ATT-02**: What if an attendance record exists for a period that was later removed from the timetable? → Record is frozen and excluded from calculations. Flagged as "Orphaned — Period Removed."
- **EC-ATT-03**: What if the same student has two attendance records for the same period on the same day? → Block at insert. Unique constraint: (student_id + date + period_number + subject_id) must be unique.
- **EC-ATT-04**: What if a correction changes a record from Present to Absent (or vice versa)? → Old value is saved in the audit log. New value replaces it. Both are visible to Principal/Admin.
- **EC-ATT-05**: What if attendance % needs to be calculated mid-semester? → Always calculated in real-time from the records. Never stored as a static value.
