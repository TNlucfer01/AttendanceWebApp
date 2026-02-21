## Staff Module

> Staff are academic employees who interact directly with students.
> Created by: [[HOD]] (approved by [[Principal]])

---

## Sub-Types of Staff

### 1. Class Advisor
> Owner of a class. Manages student records and leave requests.

**Permissions:**
1. CRUD of students in their assigned class
2. Accept or reject student **leave requests** (first-level approver)
3. Forward approved leave requests to [[HOD]] for final approval
4. View attendance of **any student** in their class
5. View leave/OD request history for their class
6. **Mark attendance** for any period in their class when the Subject Staff is absent (Staff Absent scenario — see below). The marking window is extended to **30 minutes** (instead of the usual 10) to give the advisor time to reach the class

### 2. Subject Staff
> Teaches one or more subjects. Marks attendance per period.

**Permissions:**
1. Mark attendance for their assigned subject in the **first 10 minutes** of class
2. View which students are absent and their reason (leave/OD) for that hour
3. Cannot edit attendance after the 10-minute window — must raise a request to HOD

**Attendance Marking UI Flow:**
- Default state: **All students are marked Present**
- Staff toggles only the **absent students** (tap/click to mark absent)
- Students with approved Leave/OD are pre-marked and greyed out — staff cannot override
- **Submit all at once** — not one-by-one
- Optional: Bulk CSV upload for lab sessions with different batches

---

### Note: A Staff can be BOTH a Class Advisor and a Subject Staff simultaneously

---

### Edge Cases

- **EC-STAFF-01**: What if a Subject Staff forgets to mark attendance within 10 minutes? → Attendance window closes. Staff must submit an **attendance correction request** to HOD; it cannot be edited directly.

- **EC-STAFF-02**: What if a Class Advisor rejects a leave, but the student appeals to the HOD directly? → Student can escalate **through the system** (see [[Leave]] — Escalation flow). HOD can override the rejection (logs the Class Advisor's original decision for audit). Subject to the 3-escalation-per-semester limit.

- **EC-STAFF-03**: What if a Subject Staff is assigned to a class they don't teach? → HOD must verify. System should show a warning if staff's department doesn't match the subject's department.

- **EC-STAFF-04**: What if a Class Advisor is absent on a given day? → Their class still needs attendance marked. HOD can temporarily assign another staff as acting Class Advisor for that day.

- **EC-STAFF-05**: What if the same student is marked absent by Subject Staff in Period 1 but had an approved Leave for that day? → System auto-reconciles: approved leave overrides the absent mark. Subject Staff sees "On Leave" status, not "Absent."

- **EC-STAFF-06**: What if a Staff member tries to mark attendance for a subject that is not in today's timetable? → Block it. Attendance can only be marked for subjects scheduled in the current day's timetable slot. **Note:** Cross-department subject assignments are allowed (see [[HOD]] EC-HOD-02) — if a subject is in the timetable for a class, the assigned Subject Staff can mark attendance regardless of their home department.

- **EC-STAFF-07**: What if a class has no Class Advisor assigned? → Block all student CRUD and leave operations for that class until an advisor is assigned.

---

## Staff Absent Scenario

> What happens when a Staff member is absent and has scheduled classes?

> **Scope Note:** Staff leave is **not formally tracked** in this system. Only student attendance and leave is tracked. Staff absence is handled manually — the HOD marks the staff as absent and manages the affected slots.

### When Staff is Absent — HOD Marks Them Absent and is Responsible
The HOD sees a list of all affected period slots for that day.

### HOD's Options for Each Affected Slot

| Option                      | What Happens                                                          |
| --------------------------- | --------------------------------------------------------------------- |
| **Assign Substitute Staff** | Another staff fills in and marks attendance within the 10-min window  |
| **Class Advisor Marks**     | Class Advisor marks attendance for the affected period              |
| **Mark as Free Period**     | Slot flagged as "Staff Absent — Free Period." Class Advisor still marks attendance for the hour  |

### Critical Rule — Students Still Have Attendance Counted
- When Staff is absent, the period is initially marked `SA` (Staff Absent) — a **temporary status**
- The **Class Advisor** (or substitute) must mark attendance, which resolves `SA` → `P` or `A`
- If neither marks within the 30-min extended window → `SA` auto-resolves to `NM` (Not Marked, excluded)
- The period appears in reports as "Staff Absent — Attendance by Advisor" for transparency
- HOD is notified of any unresolved `SA` slots at end of day

### Sub-cases
- **Class Advisor absent**: HOD temporarily handles leave approvals. Student CRUD is paused or HOD takes over.
- **Subject Staff absent**: Only their scheduled periods are affected. All other staff and periods run normally.
- **Staff is both Class Advisor + Subject Staff and is absent**: Both of the above apply simultaneously.

### Edge Cases
- **EC-STAFF-08**: What if the substitute assigned by HOD is already teaching another class at the same time? → Block it. System checks for timetable conflicts before confirming the substitute.
- **EC-STAFF-09**: What if a staff marks absent but then shows up? → HOD can undo the absent status. If a substitute was already assigned, HOD decides who marks attendance.
- **EC-STAFF-10**: What if a staff is absent for the entire week? → HOD must assign a permanent substitute or reassign the subject. Reports flag the subject with a "repeated absence" warning.
- **EC-STAFF-11**: What if a staff simply doesn't mark attendance (no absent recorded either)? → After the 10-min window, slot auto-closes as "Attendance Not Marked." HOD is notified and decides: exclude the period (Staff Absent) or raise an attendance correction. **Who can raise the correction:** the **Class Advisor** of that class or the **Subject Staff** assigned to that period. Both can submit a correction request to HOD for that slot. **Time limit:** Staff-raised corrections must be submitted within **14 days** of the original date (students have 7 days — staff get extra time for administrative delays).