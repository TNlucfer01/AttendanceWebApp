# Attendance System — Master Index

> A web-based college attendance management system with role-based access, leave workflows, timetable management, and automated reporting.

---

## Roles (Logins)

| Role | File | Description |
|---|---|---|
| Admin | [[admin]] | IT super-user, manages system |
| Principal | [[Principal]] | Top academic authority |
| HOD | [[HOD]] | Manages a department |
| Staff | [[Staff]] | Class Advisor or Subject Staff |
| Student | [[Student]] | Primary subject of the system |

---

## Core Entities / Modules

| Module | File | Description |
|---|---|---|
| Department | [[department]] | Groups classes under one HOD |
| ClassRoom | [[classRoom]] | A class section with students |
| Subject | [[Subject]] | A course taught in a class |
| TimeTable | [[TimeTable]] | Weekly schedule per class |
| Attendance | [[Attendance]] | Core attendance record structure and % calculation |
| Leave & OD | [[Leave]] | Leave/OD request and approval flows |
| Calendar | [[Calendar]] | Academic calendar and holiday management |
| Academic Year | [[AcadmicYear]] | Semester lifecycle and promotions |
| Notifications | [[Notifications]] | Event-driven alerts to all users |

---

## System Requirements

| Doc | File |
|---|---|
| Auth & Access Control | [[Auth]] |
| Non-Functional Requirements | [[Non-frunctional_reuirment]] |
| Edge Case Registry | [[EdgeCaseRegistry]] |

---

## Project Management

| Doc | File |
|---|---|
| Agile Methodology Basics | [[Agile_Basics]] |
| 7-Day Sprint Plan | [[Sprint_Plan]] |

---

## Key Rules (Quick Reference)

- Attendance must be marked within **10 minutes** of period start
- Default attendance threshold: **75%** (configurable by Principal)
- Leave approval: **Student → Class Advisor → HOD**
- Class creation: **HOD creates → Principal approves**
- Student count and dept count are always **computed, never stored statically**
- Documents upload is **experimental — not in initial build**

---

## Module Dependency Map

```
Principal
  └── creates → Department → HOD
                              └── creates → ClassRoom → Class Advisor (Staff)
                                                           └── manages → Students
                              └── creates → Subject → Subject Staff
                              └── creates → TimeTable (links Subject + Staff + ClassRoom)
  └── manages → Calendar (holidays, working days)
                  └── affects → TimeTable (holiday = slots cancelled)
                  └── affects → Leave (blocks leave on holidays)
  └── manages → AcadmicYear (semester lifecycle)
                  └── uses → Calendar (to compute working days)

Student
  └── submits → Leave / OD → Class Advisor → HOD (final approval)
  └── submits → Attendance Correction → Class Advisor → HOD

Attendance (core data unit)
  └── created by → Staff (Subject Staff) or Class Advisor (during SA)
  └── modified by → HOD (corrections)
  └── audited by → Principal / Admin

Academic Year (Principal)
  └── defines → Semester Lifecycle → Archive → Promote Students

Admin
  └── manages → Users, System Config, Backups, Logs
```
