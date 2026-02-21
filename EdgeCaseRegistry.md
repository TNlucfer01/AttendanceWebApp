# Edge Case Registry — Central Index

> Master list of all edge-case IDs across every module in the [[index|Attendance System]].
> Use this as a **cross-reference** to check for duplicates, conflicts, and missing coverage.

---

## Auth Module → [[Auth]]

| ID | Summary |
|---|---|
| EC-AUTH-01 | Staff is both Class Advisor and Subject Staff — merged dashboards |
| EC-AUTH-02 | Student tries to access staff URL → 403 redirect |
| EC-AUTH-03 | HOD removed mid-semester → access revoked, Admin reassigns |
| EC-AUTH-04 | Duplicate email blocked at registration |
| EC-AUTH-05 | User deleted with pending requests → flagged "User Deactivated" |
| EC-AUTH-06 | Cross-dept data access — staff can only view data of classrooms they are Class Advisor of |
| EC-AUTH-07 | Bulk CSV import with invalid data → row-by-row validation, partial import with error report |

---

## Staff Module → [[Staff]]

| ID | Summary |
|---|---|
| EC-STAFF-01 | Attendance window closed → correction request to HOD |
| EC-STAFF-02 | Student escalates rejected leave to HOD → HOD overrides (through the system, subject to 3-escalation limit) |
| EC-STAFF-03 | Staff assigned to wrong class → HOD verification + warning |
| EC-STAFF-04 | Class Advisor absent → HOD assigns acting advisor |
| EC-STAFF-05 | Approved leave overrides absent mark → auto-reconcile |
| EC-STAFF-06 | Attendance for non-scheduled subject → blocked (cross-dept subjects allowed if in timetable) |
| EC-STAFF-07 | Class has no Class Advisor → block CRUD and leave ops |
| EC-STAFF-08 | Substitute has timetable conflict → block |
| EC-STAFF-09 | Staff marked absent then shows up → HOD undoes |
| EC-STAFF-10 | Staff absent entire week → permanent substitute required |
| EC-STAFF-11 | Staff doesn't mark attendance → auto-close, HOD notified. Class Advisor or Subject Staff for that hour can raise correction |

---

## HOD Module → [[HOD]]

| ID | Summary |
|---|---|
| EC-HOD-01 | Class created but Principal unavailable → "Pending Approval" |
| EC-HOD-02 | Cross-dept Subject Staff assignment → allowed, logged clearly |
| EC-HOD-03 | Delete class with active records → blocked, must archive |
| EC-HOD-04 | Retroactive leave approval → flagged + audit-logged |
| EC-HOD-05 | Timetable conflict → auto-detected, blocked |
| EC-HOD-06 | HOD changed mid-semester → old decisions preserved |
| EC-HOD-07 | Leave limit set to 0 → minimum 1 enforced |
| EC-HOD-08 | Concurrent attendance corrections for same student/period → optimistic locking, first wins |
| EC-HOD-09 | HOD absent, no Acting HOD → approvals auto-escalate to Principal after 2 days |
| EC-HOD-10 | Acting HOD decisions disputed by returning HOD → must raise to Principal, cannot reverse unilaterally |

---

## Principal Module → [[Principal]]

| ID | Summary |
|---|---|
| EC-PRIN-01 | 0 periods per day → blocked, minimum 1 |
| EC-PRIN-02 | Dept has no HOD → block class creation until HOD assigned |
| EC-PRIN-03 | Threshold lowered mid-semester → future-only, warning shown |
| EC-PRIN-04 | Delete dept with active students → blocked |
| EC-PRIN-05 | HOD-created class rejected → HOD can edit and resubmit |
| EC-PRIN-06 | Two Principals → only 1 active allowed |
| EC-PRIN-07 | Saturday disabled → timetable slots archived, not deleted |
| EC-PRIN-08 | Dept operations blocked if no HOD assigned |
| EC-PRIN-09 | Principal absent, no delegate → all Principal-only actions blocked, Admin warned |
| EC-PRIN-10 | Saturday not enabled by Friday EOD → auto-holiday, last-resort enable Saturday morning before Period 1 |
| EC-PRIN-11 | Semester starts on Saturday → cyclic rotation starts at Monday, must be explicitly enabled |

---

## Department Module → [[department]]

| ID | Summary |
|---|---|
| EC-DEPT-01 | Two departments with same name → blocked (single college, unique by name) |
| EC-DEPT-02 | HOD resigns, no replacement → "HOD Unassigned" state, approvals escalate to Principal |
| EC-DEPT-03 | Department deleted with archived records → records retained under "Deleted Depts" archive |
| EC-DEPT-04 | Department with no name → blocked, name and code mandatory |
| EC-DEPT-05 | Class under Dept A, Subject Staff from Dept B → allowed for cross-dept subjects |
| EC-DEPT-06 | HOD of Dept A as Subject Staff in Dept B → allowed, HOD powers stay in Dept A only |

---

## Leave & OD Module → [[Leave]]

| ID | Summary |
|---|---|
| EC-LEAVE-01 | Future leave + student attends → auto-cancelled |
| EC-LEAVE-02 | Leave on holiday → blocked by calendar check |
| EC-LEAVE-03 | Class Advisor doesn't respond in 2 days → auto-escalate to HOD (does NOT count toward student's 3-escalation limit) |
| EC-LEAVE-04 | Duplicate OD for same date → blocked |
| EC-LEAVE-05 | HOD approves leave after absent mark → retroactive update + audit log |
| EC-LEAVE-06 | Leave limit exceeded → allow but flag, HOD decides |
| EC-LEAVE-07 | Partial OD (half-day) → specific period slots marked OD |
| EC-LEAVE-08 | Leave rejected but notification already sent → follow-up notification |
| EC-LEAVE-09 | Leave for date before semester start → blocked |
| EC-LEAVE-10 | Duplicate leave/OD per student per date → blocked at data-model level |
| EC-LEAVE-11 | OD event cancelled → HOD batch-revokes all linked ODs, attendance reverts OD → A |
| EC-LEAVE-12 | Principal lowers max leave limit mid-semester → applies forward only, existing leaves not clawed back |

---

## Student Module → [[Student]]

| ID | Summary |
|---|---|
| EC-STU-01 | Retroactive leave → allowed but flagged, HOD decides |
| EC-STU-02 | Attendance below threshold → auto-alert to student, parent, advisor |
| EC-STU-03 | Duplicate leave for same date → blocked |
| EC-STU-04 | Student changes dept mid-semester → old records archived |
| EC-STU-05 | Roll number changes → old archived, new assigned |
| EC-STU-06 | OD event not recognized → HOD rejects, student resubmits with proof |
| EC-STU-07 | Cross-dept subject → attendance tracked under home dept |

---

## ClassRoom Module → [[classRoom]]

| ID | Summary |
|---|---|
| EC-CLASS-01 | Class with no students → allowed, attendance disabled |
| EC-CLASS-02 | Class Advisor advises multiple sections → allowed |
| EC-CLASS-03 | Duplicate class name in same dept → blocked |
| EC-CLASS-04 | Student in two classes → blocked |
| EC-CLASS-05 | Class rejected by Principal → HOD can edit and resubmit |
| EC-CLASS-06 | Class archived mid-semester → records frozen, students reassigned |

---

## Subject Module → [[Subject]]

| ID | Summary |
|---|---|
| EC-SUB-01 | Duplicate subject code in same class → blocked |
| EC-SUB-02 | Subject Staff removed mid-semester → "Staff TBA", records remain |
| EC-SUB-03 | 0-credit audit subject → attendance still tracked |
| EC-SUB-04 | Integrated subject with separate staff → allowed |
| EC-SUB-05 | Subject deleted after attendance → blocked, must archive |
| EC-SUB-06 | Same subject code in two depts → allowed (cross-dept) |

---

## TimeTable Module → [[TimeTable]]

| ID | Summary |
|---|---|
| EC-TT-01 | Two subjects same slot → blocked |
| EC-TT-02 | Double special class → overwrite allowed with warning |
| EC-TT-03 | Subject has no staff → "Staff TBA", marking disabled |
| EC-TT-04 | Wrong period marking → blocked by real-time check |
| EC-TT-05 | Holiday → all slots cancelled, no attendance |
| EC-TT-06 | Subject hours exceed limit → warning, HOD confirms |
| EC-TT-07 | Timetable modified after attendance → past records frozen |
| EC-TT-08 | Saturday not enabled by Friday EOD → defaults to holiday, last-resort enable Saturday morning |
| EC-TT-09 | Cyclic Saturday assigns weekday with absent staff → staff-absent scenario applies |
| EC-TT-10 | HOD overrides cyclic Saturday timetable → cycle still advances to next weekday |

---

## Academic Year Module → [[AcadmicYear]]

| ID | Summary |
|---|---|
| EC-AY-01 | New semester while previous active → blocked |
| EC-AY-02 | Semester end date passed → auto-flagged "Overdue" |
| EC-AY-03 | Unresolved leave at semester end → archive blocked |
| EC-AY-04 | Dates changed while active → allowed with warning |
| EC-AY-05 | Late-joining student → attendance from join date only |
| EC-AY-06 | Student with 0 attendance → flagged for HOD review |
| EC-AY-07 | New academic year with overlapping dates → blocked by date validation |

---

## Notifications Module → [[Notifications]]

| ID | Summary |
|---|---|
| EC-NOTIF-01 | Email send failure → retry 3x, log failure |
| EC-NOTIF-02 | Duplicate notification → deduplicate within 5 min |
| EC-NOTIF-03 | Parent phone missing → skip SMS, flag profile |
| EC-NOTIF-04 | Leave cancelled after notification → follow-up sent |
| EC-NOTIF-05 | 1000+ unread → show latest 50, paginate rest |

---

## Admin Module → [[admin]]

| ID | Summary |
|---|---|
| EC-ADMIN-01 | Last admin deletion → blocked |
| EC-ADMIN-02 | Backup failure → notify admin, discard partial |
| EC-ADMIN-03 | Password reset while logged in → session invalidated |
| EC-ADMIN-04 | Staff deactivated with classes today → HOD notified |
| EC-ADMIN-05 | Multiple admins → allowed, audit-logged |

---

## Non-Functional Requirements → [[Non-frunctional_reuirment]]

| ID | Summary |
|---|---|
| EC-NFR-01 | 500 concurrent staff → queue/rate limiter, no data loss |
| EC-NFR-02 | Server down mid-semester → backup restore within 24h |
| EC-NFR-03 | Slow connection (2G) → core marking works, heavy features degrade |

---

## Calendar Module → [[Calendar]]

| ID | Summary |
|---|---|
| EC-CAL-01 | Holiday declared after attendance already marked → HOD reviews and decides to exclude or keep |
| EC-CAL-02 | Principal forgets to add holidays before semester → allow adding anytime, warning if retroactive |
| EC-CAL-03 | Two holidays for same date → block duplicate, one entry per date |
| EC-CAL-04 | Half-day declared → Principal specifies which periods cancelled, only those excluded |
| EC-CAL-05 | Government holiday on working Saturday → holiday takes precedence, Saturday slots cancelled |

---

## Attendance Module → [[Attendance]]

| ID | Summary |
|---|---|
| EC-ATT-01 | No records for a day → student not yet enrolled, count from join date |
| EC-ATT-02 | Record for removed period → frozen, excluded, flagged "Orphaned" |
| EC-ATT-03 | Duplicate record for same student/period/day → blocked by unique constraint |
| EC-ATT-04 | Correction changes Present ↔ Absent → old value in audit log |
| EC-ATT-05 | Mid-semester % calculation → always real-time, never static |

> **Note:** `SA` (Staff Absent) is a **temporary status** that resolves to P/A/NM. `SU` (Suspended) is a permanent excluded status. See [[Attendance]] for full lifecycle.
