## Department Module

> A Department groups multiple classes under one HOD.
> Created by: [[Principal]]

---

### Department Data

| Field           | Details                                            |
| --------------- | -------------------------------------------------- |
| Department Name | e.g., "Information Technology", "Computer Science" |
| Department Code | Short code e.g., "IT", "CSE", "MECH"               |
| HOD             | Linked to [[HOD]] (one HOD per dept)               |
| Total Classes   | *(Computed — do NOT store statically)*             |
| Total Students  | *(Computed — do NOT store statically)*             |
| Active Semester | Current semester lifecycle status for this dept (mirrors [[AcadmicYear]] status: Upcoming/Active/Ended/Archived) |

> ⚠️ **Student count and class count are always computed from live data, never stored as fields.** Storing them statically causes sync issues.

---

### Rules
- One department has exactly **one active HOD** at any time
- A department can span multiple years (e.g., Year 1 to Year 4)
- If a department is to be deleted, it must have **zero active students**
- Department codes must be **unique** system-wide

---

### Edge Cases
- **EC-DEPT-01**: What if two departments have the same name? → Block if name AND code are both the same. Allow same name if dept codes differ (edge case: two "IT" depts in two colleges — not applicable here since single college, so block by name too).
- **EC-DEPT-02**: What if an HOD resigns and no replacement is assigned? → Department enters "HOD Unassigned" state. Principal is notified. All HOD-level approvals are temporarily escalated to Principal.
- **EC-DEPT-03**: What if a department is deleted but still has archived attendance records? → Archived records are retained in the system under a "Deleted Depts" archive. Never hard-deleted.
- **EC-DEPT-04**: What if Principal tries to add a department with no name? → Block. Department name and code are mandatory fields.
- **EC-DEPT-05**: What if a class is under Dept A but its Subject Staff is from Dept B? → Allowed for cross-dept subjects. The department shown on the class/timetable remains Dept A.
- **EC-DEPT-06**: What if an HOD of Dept A is also assigned as Subject Staff in Dept B? → Allowed. HOD retains all HOD powers for Dept A and acts as regular Subject Staff in Dept B. System clearly labels their role per department. They cannot exercise HOD authority in Dept B.