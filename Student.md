## Student Module

> The student is the primary subject of the attendance system.
> Created by: [[Staff]] (Class Advisor)

---

### Student Profile Data

| Field        | Details                                                                                        |
| ------------ | ---------------------------------------------------------------------------------------------- |
| Roll No      | Unique identifier per student                                                                  |
| Name         | Full name                                                                                      |
| Current Year | 1st / 2nd / 3rd / 4th year                                                                     |
| Batch        | Admission year (e.g., 2022 for 2022–2026 batch)                                                |
| Department   | Linked to [[department]]                                                                       |
| Semester     | Current semester (1–8)                                                                         |
| Gender       | Male / Female / Other (for count reporting per class)                                          |
| Email        | For notifications and login                                                                    |
| Phone        | Student's phone number                                                                         |
| Parent Phone | For emergency/absentee alerts                                                                  |
| Blood Group  | Bio detail                                                                                     |
| Community    | BC / MBC / SC / ST / OC (relevant for Tamil Nadu colleges)                                     |
| Documents    | TC, Community Cert, Native Cert, Income Cert, Mark Sheets (experimental — not implemented yet) |

---

### What a Student Can Do
1. View their own **attendance summary** (overall % + per subject)
2. Submit a **Leave Request** to their Class Advisor
3. Submit an **OD Request** to their Class Advisor
4. Submit an **Attendance Correction Request** (if marked wrong by Subject Staff) → forwarded to HOD by Class Advisor
5. View status of all pending/approved/rejected requests
6. Receive **notifications** for low attendance, leave approval/rejection

### Attendance Conflict Detection
- If a student has an approved OD/Leave for a day but is also marked **Present** by a Subject Staff on the same day → system flags this as a **conflict** for HOD review
- Prevents Staff from marking friendly students as Present when they're actually absent on approved leave

### Attendance Correction Flow

```
Student notices incorrect attendance
        ↓
Student submits Attendance Correction Request (within 7 days of the original date)
  (specifying: date, period, current status, requested status, reason)
        ↓
Class Advisor reviews and forwards to HOD
        ↓ (if Class Advisor doesn't act within 3 days → auto-escalate to HOD)
        ↓ (if Class Advisor agrees)
HOD approves or rejects
        ↓ (if HOD approves)
Attendance record updated. Old value → New value logged in audit trail
        ↓ (if HOD rejects)
No change. Student is notified with rejection reason
```

> ⚠️ Correction requests must be submitted **within 7 days** of the original attendance date. After 7 days, only HOD can initiate corrections directly.

### Student Deactivation / Dropout

> What happens when a student leaves the college mid-semester (dropout, transfer, suspension).

| Action           | What Happens                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Dropout**      | Class Advisor marks student as "Dropped Out." Attendance records are frozen from that date. Student is excluded from future period counts. |
| **Transfer Out** | Same as dropout but status is "Transferred." Admin may export attendance records for the receiving institution.                            |
| **Suspension**   | HOD marks student as "Suspended" for a date range. Suspended periods are marked with `SU` status — excluded from attendance % (see [[Attendance]] — Suspended status). |
| **Re-admission** | Admin reactivates the account. Attendance restarts from the re-admission date. Old records (if any) can be linked by Admin.                |

> Deactivated students cannot log in, submit leave/OD, or appear in attendance marking lists.

---

### Edge Cases
- **EC-STU-01**: What if a student submits a leave request for a day that has already passed? → Allow it (retroactive leave), but flag it clearly. HOD approves or rejects retroactive leaves.
- **EC-STU-02**: What if a student's attendance falls below the threshold (e.g., 75%)? → Auto-alert sent to student, parent phone (SMS if integrated), and Class Advisor.
- **EC-STU-03**: What if a student submits duplicate leave requests for the same date? → Block duplicate. Show "You already have a pending/approved request for this date."
- **EC-STU-04**: What if a student changes department mid-semester? → Old attendance records are archived under old dept. New records begin fresh. Class Advisor handles the transfer.
- **EC-STU-05**: What if a student's roll number changes (e.g., due to re-admission)? → Old roll number is archived. New roll number is assigned. Attendance history from old roll number can be linked manually by Admin.
- **EC-STU-06**: What if a student submits an OD request but the event is not recognized by the HOD? → HOD can reject with a reason. Student can re-submit with supporting proof (document upload — experimental).
- **EC-STU-07**: What if a student is enrolled in a cross-department subject (e.g., Maths from Science dept)? → Attendance for that subject is tracked under the student's home department but marked by the cross-dept Subject Staff.