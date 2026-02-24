# Agile Daily Standup — Attendance Web App Sprint

This document tracks our progress using the three core Agile questions:
1. **What did I accomplish since the last update?**
2. **What will I do next?**
3. **Are there any blockers/impediments?**

---

## Day 1 — Foundation (Feb 21) ✅

### 1. What was accomplished?
- **Project Skeleton**: Initialized Spring Boot 3.2.3 project with all necessary dependencies (Security, JPA, MariaDB, Lombok, Validation).
- **Database Schema**: Implemented the full ER diagram with 20 JPA entities and 13 enums supporting roles, attendance, leave, and timetables.
- **Authentication**: Built a robust JWT-based security layer with BCrypt password hashing and custom user details service.
- **Persistence**: Switched from H2 in-memory DB to a persistent local MariaDB (MySQL) instance.
- **Bootstrapping**: Created a `DataSeeder` to automatically initialize the system with an Admin account and default configuration.
- **Infrastructure**: Initialized Git and verified the project with a full 7-point API test suite.

### 2. What will I do next?
- Implement Admin and Principal management APIs.
- Set up Department and HOD management workflows.

### 3. Are there any blockers?
- *None.* (Resolved MySQL installation during execution).

---

## Day 2 — Admin & Principal Management (Feb 22) ✅

### 1. What was accomplished?
- **Admin APIs**: Created endpoints for the Admin to spawn a Principal account and manage all system users (Active/Deactivate/Reset Password).
- **Principal APIs**: Implemented Department CRUD (Create, Read, Update, Deactivate) with uniqueness constraints.
- **HOD Management**: Developed the complex workflow to create HOD accounts and assign them to departments, handling role promotion from Staff to HOD.
- **System Config**: Enabled Principal to modify institution-level settings (periods per day, attendance thresholds, etc.).
- **RBAC**: Enforced Role-Based Access Control logic at the controller level; verified that HODs cannot access Admin functions.
- **Source Control**: Successfully published the codebase to a public GitHub repository: [TNlucfer01/AttendanceWeb](https://github.com/TNlucfer01/AttendanceWeb).

### 2. What will I do next?
- **Day 3 Goals**: HOD module development.
- Implement Classroom management (Years/Semesters).
- Implement Subject management (Theory/Lab).
- Implement Staff onboarding within departments.
- Start the Timetable management logic (Slots and Overrides).

### 3. Are there any blockers?
- *None.* (Stale class file issue in Maven was resolved with a `clean` build).

---

## Day 3 — HOD Module (Feb 23) ✅

### 1. What was accomplished?
- **Classroom Management**: Implemented full CRUD for Classrooms (III CSE A, I CSE B), including Year/Semester tracking and Class Advisor assignment.
- **Subject Management**: Developed Subject creation (Operating Systems, DBMS) and assigned Staff to Subjects.
- **Staff Management**: Enabled HODs to onboard Staff directly (Mr. Ramesh, Mr. Priya Sharma); verified role linking (STAFF/HOD) and account creation.
- **Timetable Engine**: Built the core Timetable system with weekly slots and automated **Staff Conflict Detection** (verified duplicate-booking rejection).
- **Database Refinement**: Manually resolved a MariaDB strict constraint issue by updating the `timetables` schema to allow nullable Academic Years during the MVP setup.
- **API Verification**: Successfully executed a 15-point test chain covering the entire HOD-level department setup.

### 2. What will I do next?
- **Day 4 Goals**: Student Module & Core Attendance.
- Implement Student CRUD + **Bulk CSV Import** (using `OpenCSV`).
- Developing the **Attendance Marking** API (Period-based, checksum validation).
- Initializing the **QR Session** logic for real-time marking.
- Classroom-level Attendance Dashboard for Advisors.

### 3. Are there any blockers?
- *None.*

---

## Sprint Summary Statistics
- **Progress**: 3/7 Days (approx 45% of Sprint)
- **Codebase Integrity**: 100% (All 15 HOD API tests + 7 Auth tests passing).
- **Persistence Layer**: Operational on MariaDB.
- **GitHub**: [AttendanceWeb Repository](https://github.com/TNlucfer01/AttendanceWeb)
