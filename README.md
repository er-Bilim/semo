# 🎓 Semo

**A clear, mobile-first student portal for universities in Kyrgyzstan.** Grades, schedule and attendance in one place, designed to work well on a phone. 📱

> 🚧 Status: in development (MVP)

---

## 🤔 Why

Existing university portals (like AVN) are hard to use. It's unclear where things are, they're painful on a phone, and students have to ask their group leader how the final grade is calculated. 😵‍💫

Semo takes the opposite approach:

- 🎯 **The important things come first.** Next class, new grades and outstanding debts are visible right away.
- 🧮 **Grades explain themselves.** You see what your final grade is made of and how many points you still need to reach the minimum.
- 📲 **Phone first.** It's a PWA, so it installs like an app and sends push notifications for new grades.

## ✨ Features

### 🧑‍🎓 Student
- 🏠 Home: next class (time, room, teacher), plus a summary of grades and attendance
- 🗓️ Daily and weekly schedule
- 📊 Grades by semester, broken down into Module 1, Independent Work 1, Module 2, Independent Work 2 and the exam
- 🎯 "N points left to reach the 61 minimum" hint
- ✅ Attendance per subject
- 🔔 Notifications for new grades and class reminders

### 👩‍🏫 Teacher
- 📝 Group gradebook: only the currently open assessment is editable, and totals update live
- ⚠️ At-risk students highlighted (those unlikely to reach the minimum)
- 👆 One-tap attendance marking from a phone
- 📋 To-do list: grades not entered yet, work not checked yet

### 🏛️ University admin
- 🔐 University onboarding with ownership verification
- 🌳 Structure: faculties → groups
- ⚙️ Configurable grading scheme (components, maximum points, passing minimum)
- ✉️ Invite people via group code, Excel import or email

## 👥 Roles

| Role | Access |
|---|---|
| 👑 University owner | Everything, plus appointing admins, setting the grading scheme and deleting the university |
| 🛠️ Admin (dean's office) | People, groups, schedule, semesters; can be limited to one faculty |
| 👩‍🏫 Teacher | Gradebook and attendance for their own groups and subjects only |
| 🙋 Group leader | Student access, plus attendance marking (if the teacher allows it) |
| 🧑‍🎓 Student | Their own grades, schedule and attendance only |

## 🔐 University onboarding

1. 🏗️ The owner creates a university: name, domain and work email.
2. 🪪 They prove they represent the university using one of these methods:
   - 🌐 **DNS**: a `_semo-verify` TXT record on the university domain (checked automatically);
   - 📧 **Official email**: a code sent to `admin@<university domain>`;
   - 📄 **Documents**: an official order or power of attorney, reviewed manually.
3. ⏳ Before verification, the owner can set up the structure and grading scheme but can't invite anyone.
4. 🎉 After verification, they can add teachers and students.

## 🧮 Grading model

The grading scheme is stored as data and set per university. The default:

```
Module 1             20
Independent Work 1   20
Module 2             20
Independent Work 2   20
────────────────────────
Before exam          80
Exam                 20
────────────────────────
Total               100   (passing minimum: 61)
```

## 🛠️ Tech stack

**⚙️ Backend**
- FastAPI, Pydantic v2
- PostgreSQL, SQLAlchemy 2.0 (async), Alembic
- Redis and arq for background jobs (imports, email, DNS checks)
- JWT auth: access token, plus a refresh token in an httpOnly cookie
- pytest, httpx

**🎨 Frontend**
- React, Vite, TypeScript
- TanStack Query, TanStack Router
- Tailwind CSS, Radix UI
- React Hook Form, Zod
- openapi-typescript, which generates API types from the FastAPI schema
- PWA with Web Push

**🐳 Infrastructure**
- Docker Compose, Caddy
- GitHub Actions

## 🏗️ Architecture

- 🏢 **Multi-tenancy**: every entity is tied to a `university_id`, and every query is filtered by it.
- 🛡️ **RBAC**: FastAPI dependencies check permissions by role and scope (university or faculty).
- 🕵️ **Gradebook audit log**: every grade change is recorded with its author and timestamp.

## 📁 Repository structure

```
semo/
├── backend/
│   ├── app/
│   │   ├── api/          # routes
│   │   ├── core/         # config, security, permissions
│   │   ├── models/       # SQLAlchemy models
│   │   ├── schemas/      # Pydantic schemas
│   │   ├── services/     # business logic
│   │   └── workers/      # background jobs
│   ├── alembic/
│   └── tests/
├── frontend/
│   └── src/
│       ├── api/          # generated client
│       ├── features/     # student / teacher / admin
│       ├── components/
│       └── routes/
├── docker-compose.yml
└── .env.example
```

## 🚀 Getting started

> 🔜 Coming soon. These steps will work once the Docker setup lands.

```bash
git clone https://github.com/er-Bilim/semo.git
cd semo
cp .env.example .env
docker compose up --build
```

- 🔌 API: http://localhost:8000 (docs at `/docs`)
- 💻 Client: http://localhost:5173

## 🗺️ Roadmap

**🥇 MVP**
- [ ] Auth and roles
- [ ] University onboarding with official email verification
- [ ] Structure: faculties and groups
- [ ] Teacher gradebook
- [ ] Student grades and schedule
- [ ] Responsive layout and PWA

**🔭 Next**
- [ ] DNS and document verification
- [ ] Student import from Excel
- [ ] Attendance and the group leader role
- [ ] Push notifications
- [ ] Kyrgyz and English UI

## 🎨 Design

Mockups for every screen (student, teacher, admin): [DESIGN LINK]

## 👋 Author

**Bilimbek Erkinbekov**, [@er-Bilim](https://github.com/er-Bilim)

## 📄 License

MIT