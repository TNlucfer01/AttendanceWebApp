## ClassRoom Module

> A ClassRoom (Class Section) is a group of students within a department for a specific year and semester.
> Created by: [[HOD]] — approved by [[Principal]]

---

### ClassRoom Data

| Field | Details |
|---|---|
| Class Name | e.g., "III IT A", "I CSE B" |
| Class Code | Unique identifier e.g., "IT-3-A-2024" |
| Department | Linked to [[department]] |
| Year | 1st / 2nd / 3rd / 4th |
| Semester | 1–8 |
| Class Advisor | Linked to [[Staff]] (Class Advisor sub-type) |
| Total Students (Male) | *(Computed from student records — not stored statically)* |
| Total Students (Female) | *(Computed from student records — not stored statically)* |
| Total Students (Overall) | *(Computed)* |
| TimeTable | Linked to [[TimeTable]] |
| Status | Active / Archived |

> ⚠️ **Male/Female/Total student counts are always computed live. Never stored as separate fields.**

---

### Rules
- A class can have **only one Class Advisor** at a time
- A class is linked to **one department and one year/semester** — it cannot span two years
- At semester end, the class is **archived** (not deleted)
- Each class has **exactly one timetable** per semester

---

### Edge Cases
- **EC-CLASS-01**: What if a class has no students assigned? → Allow creation. Class shows "Empty" status. Attendance marking is not possible until at least one student is added.
- **EC-CLASS-02**: What if a Class Advisor is assigned to more than one class? → Allow it (a staff can advise multiple sections). System shows all their classes on their dashboard. **Recommended max: 3 classes per advisor.** If exceeding 3, system shows a warning to HOD: "This advisor already handles [N] classes. Consider distributing the load."
- **EC-CLASS-03**: What if two classes in the same department have the same name? → Block it. Class names + department + year + semester must be unique.
- **EC-CLASS-04**: What if a student is assigned to two classes simultaneously? → Block it. A student can only belong to one active class at a time.
- **EC-CLASS-05**: What if Principal rejects a class created by HOD? → HOD is notified. Class enters "Rejected" state with rejection reason. HOD can edit and resubmit.
- **EC-CLASS-06**: What if a class is archived mid-semester due to merger? → All attendance records are frozen. Students are reassigned to another class. Old class remains archived for history.