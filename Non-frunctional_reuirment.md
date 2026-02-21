## Non-Functional Requirements

---

## High Priority

### 1. Scalability
- System must handle **500+ concurrent users** without degradation
- Database and backend must support horizontal scaling (adding more servers if needed)
- Adding new departments/classes/students must not require system downtime

### 2. Performance
- Page load time: **under 2 seconds** on a standard 4G connection
- Attendance marking API response: **under 500ms**
- Reports (PDF/Excel export) generation: **under 5 seconds** for up to 1 semester of data

### 3. Security
- All passwords stored with **bcrypt** hashing (never plain text)
- All API endpoints protected by **JWT-based authentication**
- Role-based access strictly enforced at the **API level**, not just UI level
- HTTPS only — no HTTP allowed in production
- Input validation on all forms to prevent SQL injection and XSS
- **Audit logs** stored in an **append-only** table — no UPDATE/DELETE allowed on audit records
- Admin cannot modify audit logs through the application — direct DB access is read-only for Admin
- All attendance modifications are logged (see [[HOD]] audit trail section)

### Unified Audit Log Specification
> All audit-worthy actions across the system follow a **single, standardized schema**:

| Field | Description |
|---|---|
| Timestamp | Server-side UTC timestamp of the action |
| Actor (User ID + Role) | Who performed the action |
| Action Type | e.g., ATTENDANCE_CORRECTION, LEAVE_APPROVED, STUDENT_CREATED |
| Target Entity | e.g., Student ID, Subject ID, Class ID |
| Old Value | Previous state (e.g., "Absent") |
| New Value | New state (e.g., "Present — corrected") |
| Reason / Notes | Optional text explaining why |
| Approved By | Who approved (if applicable) |

> **Storage:** Append-only table — no UPDATE or DELETE operations allowed.
> **User Viewability:** Each user can view their **own activity log** filtered to actions they performed or that affected them (see [[Notifications]] — User Activity Log section). Full cross-user audit logs are visible only to **Principal and Admin**.

### 4. Usability
- **Responsive design** — works on mobile, tablet, and desktop
- Attendance marking flow must complete in **under 3 taps/clicks** on mobile
- Error messages must be **human-readable**, not technical codes
- First-time users (staff, students) should be able to navigate without a manual

### 5. Reliability
- System uptime target: **99.5%** (less than ~44 hours downtime/year)
- Daily automated database backups (managed by [[admin]])
- If attendance marking fails mid-submission → partial data is rolled back, user is notified

---

## Low Priority

### 6. Maintainability
- Codebase must follow a **modular architecture** (separate modules per feature)
- All major functions must be documented
- Code must be version-controlled (Git)

### 7. Portability
- Must run on any standard Linux server
- No dependency on a specific cloud provider

### 8. Interoperability
- Must support **CSV import** for bulk student upload
- Must support **PDF and Excel export** for attendance reports

### 9. Accessibility
- Minimum **WCAG 2.1 Level AA** compliance
- Sufficient color contrast for readability
- All buttons and inputs must have accessible labels

### 10. Compliance
- Must not store sensitive student data (docs) unless explicitly enabled (experimental feature)
- Data retention policy: attendance records archived for minimum **5 years** after graduation

---

### Edge Cases
- **EC-NFR-01**: What if 500 staff all try to mark attendance at the exact same time (start of a period)? → System must handle this with a queue or rate limiter. No data loss allowed.
- **EC-NFR-02**: What if the server goes down mid-semester? → Backup restore must bring the system back to within the last 24 hours of data.
- **EC-NFR-03**: What if a user is on a very slow connection (2G)? → Core attendance marking must work even on a minimal data connection. Heavy features (reports, charts) can degrade gracefully.