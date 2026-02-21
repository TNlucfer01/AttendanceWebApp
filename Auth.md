## Auth Module

> Role-Based Access Control (RBAC) — every user has exactly one role.

### Roles (in order of privilege)
1. [[admin]]
2. [[Principal]]
3. [[HOD]]
4. [[Staff]] (two sub-types: Class Advisor, Subject Staff)
5. [[Student]]

---

### Login
- All users log in with **email + password**
- On first login, user must **change the default password**
- Session expires after 30 minutes of inactivity
- Only one active session per user at a time (no concurrent logins)

### User Creation
| Role | Created By |
|---|---|
| Admin | Pre-seeded (supports additional admins created by existing admin) |
| Principal | Admin |
| HOD | Principal |
| Staff | HOD (approved by Principal) |
| Student | Class Advisor Staff (bulk import via CSV or one-by-one) |

### Password
- Min 8 characters, 1 uppercase, 1 number, 1 special char
- Forgot password → OTP to registered email
- Admin can reset any user's password

### Access Control Rules
- Each role can ONLY access their own dashboard
- A Staff assigned to Dept A **cannot** view data of Dept B — **exception:** a Staff can view data of a specific classroom only if they are the **Class Advisor** of that classroom (even if it's in another department)
- A Subject Staff can only mark attendance for **their assigned subjects**
- HOD can only manage classes within **their own department**
- Principal can view **all** departments and students

---

### Edge Cases
- **EC-AUTH-01**: What if a staff member is assigned as both Class Advisor and Subject Staff? → Both roles apply; they see both dashboards merged.
- **EC-AUTH-02**: What if a student tries to access a staff URL directly? → 403 Forbidden, redirect to their own dashboard.
- **EC-AUTH-03**: What if an HOD is removed from their role mid-semester? → Their timetables and class assignments stay but access is revoked immediately. **Principal** must reassign HOD.
- **EC-AUTH-04**: What if the same email is used for two accounts? → Block at registration. Emails must be unique system-wide.
- **EC-AUTH-05**: What if a user is deleted while they have pending leave/OD requests? → Requests remain in system with a "User Deactivated" flag. Admin resolves.
- **EC-AUTH-06**: What if a Staff member is a Class Advisor for a classroom in another department? → They can view **only that specific classroom's** data, not the entire department's data. Access is scoped to the classroom, not the department.
- **EC-AUTH-07**: What if a bulk CSV student import has invalid data (duplicate roll numbers, missing fields, bad emails)? → System validates **row-by-row** before importing. Invalid rows are skipped and listed in an error report. Valid rows are imported. Class Advisor sees: "Imported [X] of [Y] students. [Z] rows failed — download error report." No partial student records are created — each row is all-or-nothing.