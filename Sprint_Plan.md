# Sprint Plan — Attendance Web App (7 Days)

> **Sprint Duration:** Feb 22 – Feb 28, 2026  
> **Team Size:** 1 developer  
> **Methodology:** [[Agile_Basics|Agile (Solo Sprint)]]  
> **Sprint Goal:** Build a functional MVP of the [[AttendanceWebApp/index|Attendance Web App]]

---

## Tech Stack (Assumed)

| Layer | Technology |
|---|---|
| Backend | Java (Spring Boot) |
| Frontend | HTML/CSS/JS  React |
| Database | MySQL|
| Auth | Spring Security + JWT |
| Build | Maven |
| Version Control | Git |

> ⚠️ Adjust the stack based on your preference. The plan structure stays the same.

---

## MoSCoW Prioritization for This Sprint

### ✅ Must Have (Days 1–5)
These are **non-negotiable** for the MVP:
- Project setup + Database schema
- [[Auth]] — Login, RBAC, session management
- [[admin]] — Pre-seeded admin, create Principal
- [[department]] — CRUD by Principal/HOD
- [[classRoom]] — CRUD by HOD, link to department
- [[Subject]] — CRUD by HOD
- [[Staff]] — CRUD by HOD, role assignment
- [[Student]] — CRUD by Class Advisor, CSV import
- [[TimeTable]] — Weekly schedule creation
- [[Attendance]] — Core marking (P, A statuses), per-period tracking
- Attendance % calculation (real-time)

### 🟡 Should Have (Days 5–6)
Important for a **complete experience** but can be simplified:
- [[Leave]] — Basic leave request + approval flow (Student → Advisor → HOD)
- [[Calendar]] — Holiday management (mark holidays, block attendance)
- Attendance correction by HOD (with audit log)
- Basic dashboard for each role

### 🔵 Could Have (Day 7 — if time permits)
Nice-to-have polish:
- [[Notifications]] — Basic alerts (low attendance warning)
- [[AcadmicYear]] — Semester lifecycle
- PDF/Excel attendance reports
- SA (Staff Absent) lifecycle automation
- Mobile-responsive UI polish

### ❌ Won't Have (This Sprint)
Explicitly deferred:
- Document upload (experimental as per [[AttendanceWebApp/index]])
- WCAG accessibility
- Email OTP for password reset
- System health monitoring dashboard
- Horizontal scaling / load testing
- Full notification system

---

## Day-by-Day Plan

### 📅 Day 1 — Foundation (Feb 22, Sat)
**Goal:** Project skeleton + database + auth

| #   | Task                                                            | Est. Hours | Status |
| --- | --------------------------------------------------------------- | ---------- | ------ |
| 1.1 | Initialize Spring Boot project (Maven, dependencies)            | 1h         | [ ]    |
| 1.2 | Design database schema (all tables, relationships, constraints) | 2h         | [ ]    |
| 1.3 | Create database + run migration scripts                         | 1h         | [ ]    |
| 1.4 | Implement [[Auth]] — Login endpoint (email + password)          | 2h         | [ ]    |
| 1.5 | Implement JWT token generation + validation                     | 1h         | [ ]    |
| 1.6 | Pre-seed Admin account                                          | 0.5h       | [ ]    |
| 1.7 | Git init + first commit                                         | 0.5h       | [ ]    |

**Deliverable:** App boots up, admin can log in, JWT works.

---

### 📅 Day 2 — Admin + Principal + Department (Feb 23, Sun)
**Goal:** Admin creates Principal, Principal creates Departments

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 2.1 | [[admin]] — Admin dashboard (minimal UI) | 1h | [ ] |
| 2.2 | Admin creates Principal account (API + UI) | 1.5h | [ ] |
| 2.3 | [[Principal]] — Principal dashboard (minimal UI) | 1h | [ ] |
| 2.4 | [[department]] — CRUD APIs (create, list, update, deactivate) | 2h | [ ] |
| 2.5 | Principal creates Department + assigns HOD | 1.5h | [ ] |
| 2.6 | Role-based route guards (RBAC middleware) | 1h | [ ] |

**Deliverable:** Full chain: Admin → Principal → Department → HOD exists.

---

### 📅 Day 3 — HOD + ClassRoom + Subject + Staff (Feb 24, Mon)
**Goal:** HOD manages their department — classes, subjects, staff

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 3.1 | [[HOD]] — HOD dashboard | 1h | [ ] |
| 3.2 | [[classRoom]] — CRUD (create class, assign Class Advisor) | 2h | [ ] |
| 3.3 | [[Subject]] — CRUD (create subject, assign Staff) | 1.5h | [ ] |
| 3.4 | [[Staff]] — CRUD (create staff, approve by Principal) | 2h | [ ] |
| 3.5 | [[Student]] — CRUD (create student, bulk CSV import) | 2h | [ ] |

**Deliverable:** HOD can fully set up a department with classes, subjects, staff, and students.

---

### 📅 Day 4 — TimeTable + Attendance Core (Feb 25, Tue)
**Goal:** The heart of the app — timetable and attendance marking

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 4.1 | [[TimeTable]] — Create weekly schedule (link Subject + Staff + Class) | 2.5h | [ ] |
| 4.2 | [[Attendance]] — Mark attendance API (per-period, per-student) | 2.5h | [ ] |
| 4.3 | Attendance status handling (P, A, IL, UL at minimum) | 1h | [ ] |
| 4.4 | Attendance % calculation (real-time query) | 1h | [ ] |
| 4.5 | Staff attendance marking UI (list students, checkboxes, submit) | 1.5h | [ ] |

**Deliverable:** Staff can create a timetable and mark attendance for students. Attendance % is calculated live.

---

### 📅 Day 5 — Student Dashboard + Leave Flow (Feb 26, Wed)
**Goal:** Students can view their attendance and request leave

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 5.1 | [[Student]] — Student dashboard (view attendance %, subject-wise) | 2h | [ ] |
| 5.2 | [[Leave]] — Leave request form (student submits) | 1.5h | [ ] |
| 5.3 | Leave approval flow: Student → Class Advisor → HOD | 2h | [ ] |
| 5.4 | Leave status reflected in attendance records (IL, UL statuses) | 1h | [ ] |
| 5.5 | [[Calendar]] — Holiday CRUD (Principal adds holidays) | 1h | [ ] |
| 5.6 | Holidays block attendance marking on those dates | 0.5h | [ ] |

**Deliverable:** Students see their attendance. Leave flow works end-to-end. Calendar has basic holidays.

---

### 📅 Day 6 — Corrections + Audit + Dashboard Polish (Feb 27, Thu)
**Goal:** HOD corrections, audit logging, and dashboard improvements

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 6.1 | [[HOD]] — Attendance correction (with reason) | 1.5h | [ ] |
| 6.2 | Audit log — log all attendance modifications | 1.5h | [ ] |
| 6.3 | Principal dashboard — view all departments, attendance summaries | 2h | [ ] |
| 6.4 | HOD dashboard — view class-wise/subject-wise attendance | 1.5h | [ ] |
| 6.5 | Low attendance alerts (highlight students below 75%) | 1h | [ ] |
| 6.6 | Password change on first login | 0.5h | [ ] |

**Deliverable:** Corrections work with audit trail. All dashboards show meaningful data. Low attendance is flagged.

---

### 📅 Day 7 — Testing + Bug Fixes + Final Polish (Feb 28, Fri)
**Goal:** Stabilize, fix bugs, make it presentable

| # | Task | Est. Hours | Status |
|---|---|---|---|
| 7.1 | End-to-end testing of all user flows | 2h | [ ] |
| 7.2 | Fix critical bugs found during testing | 2h | [ ] |
| 7.3 | UI polish — consistent styling, error messages | 1.5h | [ ] |
| 7.4 | Edge case checks (from [[EdgeCaseRegistry]]) | 1h | [ ] |
| 7.5 | Write README.md with setup instructions | 0.5h | [ ] |
| 7.6 | Final Git commit + tag as v1.0-MVP | 0.5h | [ ] |

**Deliverable:** A stable, demonstrable MVP. Ready for review or demo.

---

## Sprint Board Template

Track your progress daily using this simple board:

### To Do
- [ ] List tasks from today's plan here each morning

### In Progress
- [ ] Move tasks here when you start working on them

### Done
- [x] Move tasks here when completed and tested

---

## Daily Standup Template

Copy this into your daily note each morning:

```markdown
## Standup — [Date]
### ✅ Yesterday
- (what you completed)

### 🎯 Today
- (what you plan to do)

### 🚧 Blockers
- (anything stuck or unclear)
```

---

## Sprint Review Checklist (Day 7 End)

At the end of the sprint, answer these:

- [ ] Can Admin log in and create a Principal?
- [ ] Can Principal create Departments and assign HODs?
- [ ] Can HOD create Classes, Subjects, Staff, and Students?
- [ ] Can Staff create a Timetable and mark attendance?
- [ ] Is attendance % calculated correctly?
- [ ] Can Students view their attendance?
- [ ] Can Students request leave?
- [ ] Does the leave approval chain work?
- [ ] Does attendance correction with audit log work?
- [ ] Are holidays blocking attendance?

> If **8 out of 10** are checked, the sprint is a **success**. 🎉

---

## Tips for 7-Day Solo Sprint

1. **Don't over-design the UI** — Functional > Beautiful for MVP
2. **Backend first, frontend second** — APIs before screens
3. **Test as you build** — Don't save all testing for Day 7
4. **Git commit every 2-3 hours** — Small, frequent commits
5. **If stuck for 30+ min** — Move on, come back later with fresh eyes
6. **Protect Day 7** — Don't add new features on the last day, only fix and polish

---

## Related
- [[Agile_Basics]] — Agile methodology fundamentals
- [[AttendanceWebApp/index]] — Project master index
- [[EdgeCaseRegistry]] — Known edge cases to address
