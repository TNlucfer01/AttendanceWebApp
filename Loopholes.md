# Loopholes — All Fixed ✅

> Last audited: 21-Feb-2026
> All 20 loopholes fixed: 21-Feb-2026

---

## 🔴 Critical — All Fixed

| # | Loophole | Fix Applied |
|---|---|---|
| L-01 | SA status ambiguity | Defined SA as temporary status with lifecycle (SA → P/A or SA → NM). Added to `Attendance.md` |
| L-02 | Suspension comparison wrong | Changed "similar to Staff Absent" → uses new `SU` status in `Student.md` |
| L-03 | No Suspended status in enum | Added `SU` (Suspended) status to `Attendance.md` enum — excluded from total |
| L-04 | Class Advisor can't mark SA periods | Added permission #6 to Class Advisor + 30-min extended window in `Staff.md` |
| L-05 | Correction flow — no escalation | Added 3-day auto-escalation to HOD in `Student.md` flow diagram |
| L-06 | EC-AUTH-03 says Admin reassigns HOD | Changed to "Principal must reassign HOD" in `Auth.md` |

---

## 🟡 Medium — All Fixed

| # | Loophole | Fix Applied |
|---|---|---|
| L-07 | OD event cancellation | Added EC-LEAVE-11 — HOD batch-revokes, attendance reverts OD → A |
| L-08 | No Class Advisor limit | Recommended max 3 with warning in EC-CLASS-02 (`classRoom.md`) |
| L-09 | Staff correction no time limit | Added 14-day limit for staff in EC-STAFF-11 (`Staff.md`) |
| L-10 | Mid-semester leave limit change | Added EC-LEAVE-12 — forward-only, no clawback (`Leave.md`) |
| L-11 | EC-TT-05 Admin/Principal inconsistency | Fixed to "Principal" only + clarified Calendar.md header/table |
| L-12 | Partial OD ambiguity | Clarified EC-LEAVE-07 — period-by-period, each retains own status |
| L-13 | Academic year date overlap | Added EC-AY-07 — overlapping dates blocked (`AcadmicYear.md`) |
| L-14 | No parent notification on deactivation | Added notification trigger in `Notifications.md` |
| L-15 | HOD self-approval for leave | Extended Self-Approval Block — leave escalates to Principal (`HOD.md`) |

---

## 🟢 Minor — All Fixed

| # | Loophole | Fix Applied |
|---|---|---|
| L-16 | Department "Active Semester" confusing | Clarified — mirrors AcadmicYear lifecycle status (`department.md`) |
| L-17 | HOD as Subject Staff in another dept | Added EC-DEPT-06 — allowed, HOD powers stay in home dept |
| L-18 | Dependency map incomplete | Updated `index.md` — added Calendar, Attendance, and all relationships |
| L-19 | 7-day rule missing from diagram | Added "(within 7 days)" to correction flow in `Student.md` |
| L-20 | Bulk CSV import failures | Added EC-AUTH-07 — row-by-row validation, partial import (`Auth.md`) |
