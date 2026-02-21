## Subject Module

> A Subject represents a course taught in a specific class during a semester.
> Created by: [[HOD]]

---

### Subject Data

| Field | Details |
|---|---|
| Subject Name | e.g., "Neural Networks and Deep Learning" |
| Subject Code | Unique code e.g., "CCS355" |
| Subject Type | Theory / Practical / Lab+Theory (Integrated) |
| Credits | Number of academic credits (can be 0 for audit subjects) |
| Semester | Semester number (1–8) |
| Department | Linked to [[department]] |
| Class | Linked to [[classRoom]] |
| Assigned Staff | Linked to [[Staff]] (Subject Staff) |
| Hours Per Week | Total periods allocated in timetable |

---

### Rules
- A subject can be assigned to **only one class** per semester
- A subject with type **Lab+Theory (Integrated)** is counted as **two entries in the timetable** (theory hour + lab hour) but shares a single subject record
- A subject can have **no assigned staff** temporarily (shown as "Staff TBA")
- Cross-department subjects are allowed (e.g., English taught to CSE class by English dept staff)

---

### Edge Cases
- **EC-SUB-01**: What if the HOD assigns two subjects with the same subject code to the same class? → Block it. Subject codes must be unique per department per semester.
- **EC-SUB-02**: What if a Subject Staff is removed mid-semester? → Subject shows "Staff TBA." HOD must reassign. All past attendance records remain intact under the old staff ID.
- **EC-SUB-03**: What if a subject has 0 credits (audit-only)? → Allow it, but attendance is still tracked normally. The subject appears in reports with credits marked as "Audit."
- **EC-SUB-04**: What if an Integrated (Lab+Theory) subject has separate staff for theory and lab? → Allow separate staff assignment for theory and lab slots. Both are linked to the same subject record.
- **EC-SUB-05**: What if a subject is deleted after attendance has already been marked for it? → Block deletion. HOD can **archive** the subject instead. Archived subjects retain all attendance records.
- **EC-SUB-06**: What if the same subject code exists in two departments? → Allowed (cross-dept subjects have the same code globally). Uniqueness is enforced per class, not system-wide.
