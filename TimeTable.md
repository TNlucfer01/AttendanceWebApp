## TimeTable Module

> Each [[classRoom]] has one active timetable per semester.
> Created and managed by: [[HOD]]

---

### Timetable Structure

A timetable is a **weekly schedule** made up of **period slots**.

#### Period Slot Fields
| Field | Details |
|---|---|
| Day | Monday – Saturday (Saturday optional, enabled by [[Principal]]) |
| Period Number | 1, 2, 3... up to max set by Principal |
| Start Time | e.g., 09:00 |
| End Time | e.g., 09:50 |
| Type | Regular / Break / Lunch |
| Subject | Linked to [[Subject]] (null for Break/Lunch slots) |
| Staff | Linked to [[Staff]] for that slot |

---

### Rules
- **No two period slots** can have the same staff at the same time across any classes (conflict check)
- HOD can add a **Special Class** for any slot — overrides the regular subject for that day only (one-time change)
- Saturday timetable is only available if Principal has **enabled it for that specific week** (see [[Principal]] — Saturday Working Day section)
- When Saturday is enabled, the timetable is **auto-assigned** using a cyclic weekday rotation (Mon→Tue→Wed→Thu→Fri→Mon...)
- HOD can override the auto-assigned Saturday timetable for any specific Saturday
- Total period hours per week per subject must not exceed the **"Hours Per Week"** defined in [[Subject]]
- Break and Lunch slots are **non-attendance** slots — no attendance can be marked during these

---

### How Attendance Marking Works with Timetable
- When a Subject Staff opens the attendance page, the system shows **only today's scheduled subjects** for their class
- The staff can mark attendance only within the **first 10 minutes** of the period start time
- After 10 minutes, the marking window closes and shows as "Window Closed"
- **All time checks use server-side UTC** — client device clock is ignored to prevent skew issues
- **"Late Start" override:** HOD can shift the marking window by N minutes for a specific period on a specific day (e.g., if Period 2 starts 5 minutes late, HOD extends window by 5 min)

### Unmarked Attendance Slots
- If no one marks attendance for a period and HOD does not act → the slot stays as **"Attendance Not Marked"**
- **"Not Marked" = period is EXCLUDED** from the total period count (only if no one — not even Class Advisor — marks attendance)
- **"Staff Absent" ≠ "Not Marked"** — when staff is absent, the **Class Advisor marks attendance** for that period, so it still counts toward the total (see [[Staff]] — Staff Absent Scenario)
- HOD sees a dashboard item: **"N slots unmarked this week"** — they can then decide to exclude permanently or raise an attendance correction

---

### Edge Cases
- **EC-TT-01**: What if the HOD creates a timetable with two subjects in the same period for the same class? → Block it. One slot = one subject only.
- **EC-TT-02**: What if a Special Class is added for a slot that already has a Special Class? → Overwrite allowed. HOD sees a warning: "This slot already has a special override."
- **EC-TT-03**: What if a period's subject is not yet assigned a staff? → Timetable slot shows "Staff TBA." Attendance marking is disabled for that slot until staff is assigned.
- **EC-TT-04**: What if a Subject Staff marks attendance for the wrong period (e.g., Period 2 when Period 1 is active)? → Block it. Only the currently active period (based on real time) is available for marking.
- **EC-TT-05**: What if a college has a holiday on a particular day? → **Principal** can mark the day as a holiday (see [[Calendar]]). All period slots for that day are cancelled; attendance is not required.
- **EC-TT-06**: What if the total scheduled hours for a subject exceed the subject's "Hours Per Week"? → Show a warning when saving. HOD must confirm the override.
- **EC-TT-07**: What if the timetable is modified after attendance has already been marked for some slots? → Past attendance records are frozen. Only future slots are updated.
- **EC-TT-08**: What if Saturday is enabled but Principal didn't do it by Friday EOD? → Saturday defaults to holiday. Principal can still enable it on Saturday morning (before Period 1 start time) as a last resort, but system shows a warning.
- **EC-TT-09**: What if the cyclic Saturday timetable assigns a weekday whose Subject Staff is on leave? → Same as any staff-absent scenario — HOD assigns substitute or Class Advisor marks attendance (see [[Staff]] — Staff Absent Scenario).
- **EC-TT-10**: What if HOD overrides the cyclic Saturday timetable — does the cycle still advance? → Yes. The cycle tracks which weekday was *supposed* to be next, regardless of overrides. Next Saturday uses the next day in sequence.