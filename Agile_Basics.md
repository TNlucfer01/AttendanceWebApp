# Agile Methodology — Basics for Solo Developers

> A practical introduction to Agile for building the [[index|Attendance Web App]] as a **solo developer** within a **7-day sprint**.

---

## What is Agile?

Agile is a **flexible, iterative** approach to building software. Instead of planning everything upfront and building the whole thing at once (Waterfall), you:

1. **Break** the project into small, working pieces
2. **Build** one piece at a time (in short cycles called **sprints**)
3. **Test** each piece immediately after building
4. **Review** what worked and what didn't
5. **Adapt** your plan based on real progress

> 💡 Agile was originally designed for teams, but the principles work beautifully for solo developers too — in fact, the tight feedback loop is even faster when you're alone.

---

## Core Agile Concepts

### 1. Sprint
A **fixed time period** where you work on a set of features.

| Term | Meaning |
|---|---|
| Sprint Length | For us: **7 days** (Feb 22 – Feb 28, 2026) |
| Sprint Goal | A working MVP of the Attendance Web App |
| Daily Standup | A 5-minute self-check each morning (see below) |

### 2. User Stories
Instead of writing technical specs first, Agile describes features from the **user's perspective**:

```
As a [role], I want to [action], so that [benefit].
```

**Examples from our project:**
- *As a **Staff member**, I want to mark attendance for my assigned subject, so that attendance records are created per-period.*
- *As a **Student**, I want to view my attendance %, so that I know if I'm below the 75% threshold.*
- *As an **HOD**, I want to approve leave requests, so that students can take authorized leave.*

### 3. Product Backlog
A **prioritized list** of all user stories / features you want to build. You pick the **top items** for each sprint.

### 4. Sprint Backlog
The **subset** of the product backlog that you commit to completing in the current sprint.

### 5. Definition of Done (DoD)
A feature is "done" **only** when:
- [x] Code is written and compiles
- [x] Basic UI is functional
- [x] Core logic works (tested manually)
- [x] No critical bugs
- [x] Data is saved correctly to the database

### 6. Daily Standup (Solo Version)
Every morning, answer these **3 questions** in a notebook or file:
1. ✅ What did I complete yesterday?
2. 🎯 What will I work on today?
3. 🚧 What's blocking me?

> This takes 5 minutes and keeps you honest about progress.

---

## Agile Workflow for Solo Developer

```
Day Start
  └── Daily Standup (5 min self-check)
         ↓
  └── Pick tasks from Sprint Backlog
         ↓
  └── Build (code the feature)
         ↓
  └── Test (does it work? any bugs?)
         ↓
  └── Commit (save your progress with Git)
         ↓
  └── Review (end of day — update task board)
         ↓
Day End
```

---

## Agile Tools (Simple & Free)

| Tool | Purpose | Recommendation |
|---|---|---|
| **Task Board** | Track what's To Do / In Progress / Done | Use a simple Obsidian note or Trello (free) |
| **Git** | Version control — save your code history | GitHub / GitLab (free) |
| **Daily Log** | Track daily standups | An Obsidian daily note |
| **Timer** | Pomodoro technique for focus | 25 min work / 5 min break |

---

## Key Agile Principles for You

1. **Working software > Perfect documentation** — Get things running first, polish later
2. **Respond to change > Following a rigid plan** — If something takes longer, re-prioritize
3. **Deliver frequently** — Every day should produce something visible and testable
4. **Simplicity** — Build the simplest thing that works, then iterate
5. **Sustainable pace** — Don't burn out. 6-8 focused hours/day is realistic for solo work

---

## MoSCoW Prioritization

Categorize every feature as:

| Priority | Meaning | Rule |
|---|---|---|
| **Must Have** | Core features — app is useless without these | Build these first |
| **Should Have** | Important but not critical for day 1 | Build if time allows |
| **Could Have** | Nice-to-have — improves UX | Build only if ahead of schedule |
| **Won't Have** | Out of scope for this sprint | Explicitly skip (don't feel guilty) |

---

## Common Solo Developer Mistakes

| ❌ Mistake | ✅ Fix |
|---|---|
| Trying to build everything at once | Focus on 1 module at a time |
| Perfecting UI before logic works | Get logic working → then make it pretty |
| Not committing to Git daily | Commit at least once per day, even WIP |
| Skipping testing | Test each feature immediately |
| Not tracking progress | Update your task board daily |
| Working 12+ hours without breaks | Use Pomodoro: 25 min on, 5 min off |

---

## Related
- [[Sprint_Plan]] — The 7-day project plan for this app
- [[index]] — Project master index
