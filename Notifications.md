## Notifications Module

> Auto-alerts sent to users based on system events.
> This is **not a messaging system** — it is one-way event-driven alerts.

---

### Notification Triggers

| Event                                                | Who Gets Notified                                                |
| ---------------------------------------------------- | ---------------------------------------------------------------- |
| Student attendance falls below threshold (e.g., 75%) | Student + Parent (SMS/email) + Class Advisor                     |
| Leave request submitted                              | Class Advisor                                                    |
| Leave approved by Class Advisor                      | Student + HOD                                                    |
| Leave rejected by Class Advisor                      | Student                                                          |
| Leave approved by HOD                                | Student + Parent                                                 |
| Leave rejected by HOD                                | Student                                                          |
| OD request submitted                                 | Class Advisor                                                    |
| OD approved (final)                                  | Student                                                          |
| OD rejected (final)                                  | Student                                                          |
| Leave request not acted on within 2 days             | HOD (auto-escalation alert)                                      |
| Attendance correction request raised                 | HOD + Class Advisor of that class + Subject Staff of that period |
| Attendance correction approved/rejected              | Student + Class Advisor + Subject Staff who raised it            |
| Semester ended / attendance archived                 | All Staff + HODs in that dept                                    |
| Student promoted to next semester                    | Student                                                          |
| New class or subject created (pending approval)      | Principal                                                        |
| Class/Subject creation approved or rejected          | HOD                                                              |
| Staff account deactivated                            | HOD of that staff's dept                                         |
| Substitute staff assigned to a period                | The substitute staff member                                      |
| Student attendance recovers above threshold          | Student + Class Advisor ("Attendance now above [X]%")            |
| Retroactive leave approved                           | Student + Parent ("Leave approved for [past date]")              |
| Student deactivated (dropout/suspension/transfer)    | Parent (SMS + email) + Class Advisor + HOD                       |
| System error / downtime                              | Admin                                                            |

---

### Delivery Methods
- **In-app notification bell** (all users)
- **Email** (all users — for important events)
- **SMS** *(future / optional — only for student low attendance alerts to parent)*

---

### Rules
- Notifications are **read/unread** tracked
- Notifications older than **6 months** are auto-archived (not deleted)
- Users can **not** turn off critical notifications (e.g., low attendance, semester end)
- Users **can** mute non-critical notifications (e.g., timetable updates)

### User Activity Log (Audit Trail)
> Each user can view their own **activity log** — a read-only history of all actions they performed or that affected them.

| Role                  | What They See in Their Activity Log                                              |
| --------------------- | -------------------------------------------------------------------------------- |
| Student               | Attendance changes, leave/OD submissions & outcomes, low-attendance alerts       |
| Staff (Subject)       | Attendance submissions, correction requests raised                               |
| Staff (Class Advisor) | Student CRUD actions, leave approvals/rejections, forwarded requests             |
| HOD                   | All approvals, attendance corrections, timetable changes, substitute assignments |
| Principal             | Semester lifecycle actions, threshold changes, department/HOD operations         |
| Admin                 | Password resets, account activations/deactivations, backup/restore actions       |

> ⚠️ Users can **view** their own logs but **cannot edit or delete** them. Full audit logs (cross-user) are visible only to **Principal and Admin** (see [[HOD]] audit trail section and [[Non-frunctional_reuirment]] security section).

---

### Edge Cases
- **EC-NOTIF-01**: What if an email fails to send? → Retry up to 3 times. If all fail, log the failure and show it in Admin's system logs.
- **EC-NOTIF-02**: What if the same event triggers a notification twice (duplicate)? → Deduplicate — same event + same user + within 5 minutes = only one notification sent.
- **EC-NOTIF-03**: What if a student's parent phone number is not registered? → Skip SMS. Send email to student only. Flag the student profile as "Parent contact missing."
- **EC-NOTIF-04**: What if a notification is sent for a leave that was later cancelled? → Send a follow-up "Leave Cancelled" notification to all previously notified parties.
- **EC-NOTIF-05**: What if a user has 1000+ unread notifications? → Show latest 50. Add a "Mark all as read" button. Older ones are paginated.
