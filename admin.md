## Admin Module

> The Admin is the IT super-user. The system is **pre-seeded with one admin** but supports additional admins if needed.
> Admin is not a college staff — they manage the system itself.

### Responsibilities
1. Create and manage the Principal account
2. Reset passwords for any user
3. Manage system-level configurations (e.g., email/SMTP settings, backup schedule)
4. View system logs and error reports
5. Deactivate or reactivate any user account
6. Backup and restore the database
7. Monitor system health (uptime, error rate, concurrent users)

### What Admin CANNOT Do
- Admin **cannot** modify attendance records
- Admin **cannot** create HOD, Staff, or Student accounts directly (those are done by their respective superiors)
- Admin **does not** have access to student academic data

---

### Edge Cases
- **EC-ADMIN-01**: What if the last Admin account is deleted or locked? → System blocks deletion/deactivation of the last admin account. There must always be at least 1 active admin.
- **EC-ADMIN-02**: What if there is a database failure during backup? → System notifies admin via email and logs the failure. Partial backups are discarded.
- **EC-ADMIN-03**: What if the Admin resets a user's password while that user is logged in? → Current session is invalidated. User is forced to log in again with the new password.
- **EC-ADMIN-04**: What if the Admin deactivates a Staff account that has classes scheduled today? → HOD is notified immediately. Classes show "No Staff Assigned" until reassigned.
- **EC-ADMIN-05**: What if additional Admins are needed? → System supports multiple admin accounts. Each new admin must be created by an existing admin. All admin actions are audit-logged.
