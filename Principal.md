## Principal Module

> The Principal is the highest academic authority in the system.
> Created by: [[admin]]

### Configuration Powers (System Setup)
1. Set number of periods per day
2. Set working days (Mon–Fri or Mon–Sat)
   - **Saturday Working Day** — see dedicated section below
3. Set default attendance threshold (default: **75%**) — per dept or system-wide
4. Create departments and assign HODs
5. Approve or reject HOD-created classes and class advisors — **Principal does NOT directly create classes**; only HOD creates, Principal approves
6. Set system-wide **max leave limit per semester** (e.g., 15 days). HODs can set their department limit anywhere from 1 to this max
7. Set system-wide **max OD days per semester** (default: 10 days). HODs can request exceptions for specific students

### Attendance & Academic Oversight
8. View attendance of any individual student across all departments
9. View attendance summary for any class, department, or semester
10. Export attendance reports (PDF / Excel) — daily, weekly, monthly

### Academic Year Management
11. Create and start a new [[AcadmicYear]]
12. Approve semester-end (triggers attendance archive and student promotion)

---

### Saturday Working Day — Weekly Enable

> Saturdays are **not permanently on/off**. Principal enables or disables each Saturday **individually, before that day**.

#### Rules
- Principal must decide **by Friday end-of-day** whether the upcoming Saturday is a working day
- If not enabled by Friday EOD → Saturday is automatically treated as a **holiday** for that week
- When enabling a Saturday, the system auto-assigns a timetable using the **cyclic rotation** rule (see below)
- Principal can override the auto-assigned timetable for any specific Saturday

#### Cyclic Saturday Timetable
- Saturday follows a **rotating weekday timetable**: Monday → Tuesday → Wednesday → Thursday → Friday → Monday → ...
- Example:
  - Saturday Week 1 → **Monday's timetable**
  - Saturday Week 2 → **Tuesday's timetable**
  - Saturday Week 3 → **Wednesday's timetable**
  - Saturday Week 4 → **Thursday's timetable**
  - Saturday Week 5 → **Friday's timetable**
  - Saturday Week 6 → **Monday's timetable** (cycle repeats)
- The cycle is tracked per class — system remembers which weekday was last used
- If a Saturday is skipped (holiday/disabled), the cycle **does not advance** — next enabled Saturday uses the same weekday that was skipped
- All Subject Staff assigned to the cycled weekday are responsible for attendance on that Saturday

> ⚠️ The cyclic timetable is auto-generated, but HOD can override any specific Saturday's timetable if needed.

---

### Principal Leave / Absence

> The Principal is the highest academic authority. Their absence must not block critical operations.

#### When Principal is Absent

| Situation | What Happens |
|---|---|
| **HOD-created class pending approval** | Stays in "Pending" until Principal returns. Cannot be auto-approved. |
| **Semester start/end actions** | Blocked. Only Principal can trigger these. |
| **Saturday enable for next week** | Must be done before Principal leaves, or that Saturday defaults to holiday. |
| **Leave limit / threshold changes** | Blocked until Principal returns. |
| **Attendance reports / viewing** | HODs can still generate their own departmental reports. |

#### Emergency Delegation
- Admin can flag an HOD as **"Principal Delegate"** for a defined period
- Delegate can: approve/reject classes, enable Saturdays, view cross-department reports
- Delegate **cannot**: start/end semesters, change system-wide thresholds, or create departments
- All delegate actions are audit-logged with a "Principal Delegate" tag
- When Principal returns, delegate access is automatically revoked

---

### Edge Cases
- **EC-PRIN-01**: What if the Principal sets 0 periods per day? → System must enforce a minimum of 1 period per day. Show error.
- **EC-PRIN-02**: What if a department has no HOD assigned and Principal tries to add a class? → Block class creation. HOD must be assigned first.
- **EC-PRIN-03**: What if the Principal lowers the attendance threshold mid-semester from 75% to 60%? → The new threshold applies going forward. Already-generated alerts are NOT retroactively removed. Show a warning before saving.
- **EC-PRIN-04**: What if a Principal tries to delete a department that has active students? → Block deletion. Must transfer or graduate all students first.
- **EC-PRIN-05**: What if the Principal rejects an HOD-created class? → HOD is notified. Class remains in "Pending" state; HOD can edit and resubmit.
- **EC-PRIN-06**: What if two Principals are added (e.g., Admin error)? → System should allow only 1 active Principal at a time. Second one must be set to "Vice Principal" (or a future role).
- **EC-PRIN-07**: What if Saturday is enabled as a working day for one semester and then disabled? → Timetable slots for Saturday are hidden/archived, not deleted. Future reactivation restores them.
- **EC-PRIN-08**: What if a department exists but has no HOD assigned? → **All department-level operations are blocked** (class creation, subject assignment, timetable creation) until an HOD is assigned. Principal is shown a warning: "Department [X] has no HOD. Assign one to proceed."
- **EC-PRIN-09**: What if the Principal is absent for an extended period with no delegate assigned? → All Principal-only actions are blocked. Admin is shown a warning: "Principal is inactive. Assign a delegate to prevent operational blocks."
- **EC-PRIN-10**: What if the Principal forgets to enable Saturday by Friday EOD? → Saturday auto-defaults to holiday. If Principal enables on Saturday morning before Period 1, system allows it with a warning. After Period 1 start time, Saturday is locked as holiday.
- **EC-PRIN-11**: What if the semester starts on a Saturday? → The cyclic rotation begins at Monday's timetable. Principal must explicitly enable that Saturday before the semester starts.