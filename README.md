<div align="center">

# 🌍 AIU Global Hub

**A full-stack web platform for AIU students to discover and apply for international internships, exchange programs, and dual-degree opportunities.**

[![Node.js](https://img.shields.io/badge/Node.js-≥18-339933?style=flat-square&logo=node.js&logoColor=white)](https://nodejs.org)
[![Express](https://img.shields.io/badge/Express-4.x-000000?style=flat-square&logo=express&logoColor=white)](https://expressjs.com)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57?style=flat-square&logo=sqlite&logoColor=white)](https://sqlite.org)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)](LICENSE)

[Overview](#-overview) · [Features](#-features) · [Tech Stack](#-tech-stack) · [Quick Start](#-quick-start) · [API Reference](#-api-reference) · [Pages](#-pages) · [Database](#-database-schema)

</div>

---

## 📌 Overview

**AIU Global Hub** is a multi-page web application built for **Ahram International University (AIU)** students and administrators. It allows students to browse global academic opportunities, submit applications, and track their status — while admins and managers can manage listings and review applicants.

The backend is a lightweight **Node.js + Express** REST API backed by **SQLite**, serving both the API and static frontend files from a single server process.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🔐 **Authentication** | Register / Login with AIU email (`@aiu.edu.eg`) only |
| 🔒 **Password security** | Passwords hashed with bcrypt (10 salt rounds) |
| 👤 **Role-based access** | `student`, `admin`, and `manager` roles |
| 🌐 **Opportunities** | Browse internships, exchange semesters, and dual-degree programs |
| 📋 **Applications** | Students submit applications with personal statement and details |
| 🛡️ **Manager guard** | Create / edit / delete opportunities locked to admin or manager roles |
| 🚀 **Auto-seeding** | Database seeds demo users and 5 real AIU partner opportunities on first start |
| 📄 **20+ pages** | Full multi-page frontend with student and admin dashboards |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML · CSS · Vanilla JavaScript |
| Backend | Node.js · Express 4 |
| Database | SQLite 3 (file-based, zero config) |
| ORM / Driver | `sqlite3` npm package (Promise-wrapped) |
| Auth | `bcrypt` password hashing |
| Dev tooling | `nodemon` (hot reload) |

---

## 📁 Project Structure

```
webproj/
├── backend/
│   ├── db.js           # Promise wrappers around sqlite3 (run, get, all)
│   ├── initDb.js       # Creates tables + seeds demo users and opportunities
│   ├── server.js       # Express app — routes, middleware, static file serving
│   ├── database.sqlite # Auto-created SQLite database file
│   └── package.json
│
└── webproj/            # Frontend (served as static files by Express)
    ├── index.html              # Home / landing page
    ├── login.html              # Login page
    ├── register.html           # Registration page
    ├── opportunities.html      # Browse all opportunities
    ├── internship-details.html # Single opportunity detail
    ├── application-form.html   # Submit an application
    ├── student-dashboard.html  # Student's personal dashboard
    ├── admin-dashboard.html    # Admin overview
    ├── opportunity-manager.html # Create / edit / delete listings
    ├── user-accounts.html      # User management (admin)
    ├── reports.html            # Reports view
    ├── system-settings.html    # System settings (admin)
    ├── exchange-programs.html  # Exchange programs page
    ├── internships-abroad.html # International internships
    ├── aiu-research.html       # AIU research opportunities
    ├── global-map.html         # Global opportunities map
    ├── how-it-works.html       # Info page
    ├── about.html              # About AIU Global Hub
    ├── resources.html          # Student resources
    ├── success-stories.html    # Alumni success stories
    └── js/
        └── api.js              # Shared fetch wrapper + auth helpers
```

---

## 🚀 Quick Start

### Prerequisites

- [Node.js](https://nodejs.org) ≥ 18

### 1 — Clone the repository

```bash
git clone https://github.com/your-username/aiu-global-hub.git
cd aiu-global-hub/backend
```

### 2 — Install dependencies

```bash
npm install
```

### 3 — Start the server

```bash
# Development (auto-restart on file changes)
npm run dev

# Production
npm start
```

The server starts on **http://localhost:3000**.

On first run, `initDb.js` automatically:
- Creates the `users`, `opportunities`, and `applications` tables
- Seeds two demo accounts
- Seeds 5 real AIU partner opportunities

### 4 — Open the app

Visit **http://localhost:3000** in your browser.

---

## 🔑 Demo Accounts

| Role | Email | Password |
|---|---|---|
| Admin | `admin@aiu.edu.eg` | `123456` |
| Student | `student@aiu.edu.eg` | `123456` |

> All new registrations must use an `@aiu.edu.eg` email address.

---

## 📡 API Reference

**Base URL:** `http://localhost:3000/api`

### Auth

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/auth/register` | Register a new student account | Public |
| `POST` | `/auth/login` | Login and receive user object | Public |

### Opportunities

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `GET` | `/opportunities` | List all opportunities | Public |
| `GET` | `/opportunities/:id` | Get a single opportunity | Public |
| `POST` | `/opportunities` | Create a new opportunity | Admin / Manager |
| `PUT` | `/opportunities/:id` | Update an opportunity | Admin / Manager |
| `DELETE` | `/opportunities/:id` | Delete an opportunity | Admin / Manager |

### Applications

| Method | Endpoint | Description | Auth |
|---|---|---|---|
| `POST` | `/applications` | Submit a new application | Student |
| `GET` | `/applications/student/:userId` | Get a student's applications | Student |
| `GET` | `/applications` | Get all applications | Admin / Manager |

### Health

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | API health check |

---

### Request Body Examples

**Register**
```json
{
  "name": "Sara Ahmed",
  "email": "sara@aiu.edu.eg",
  "password": "yourpassword",
  "universityId": "20210012"
}
```

**Create Opportunity** *(admin / manager only)*
```json
{
  "title": "Ontario Tech Research Internship",
  "category": "Internship",
  "field": "Computer Science",
  "location": "Oshawa, Ontario, Canada",
  "provider": "Ontario Tech University",
  "term": "Summer 2026",
  "description": "Work with an international research team on AI and software prototypes.",
  "requirements": "GPA 3.0+; Python or JavaScript experience.",
  "status": "active"
}
```

**Submit Application**
```json
{
  "userId": 3,
  "opportunityId": 1,
  "statement": "I am passionate about AI research...",
  "recommendationProfessor": "Dr. Mohamed Hassan",
  "graduationYear": "2026"
}
```

---

### Error Responses

| Status | Meaning |
|---|---|
| `400` | Missing or invalid fields |
| `401` | Wrong email or password |
| `403` | Insufficient role (non-manager trying to manage opportunities) |
| `404` | Resource not found |
| `409` | Conflict — duplicate email or duplicate application |
| `500` | Internal server error |

---

## 🗄️ Database Schema

**File:** `backend/database.sqlite` (auto-created)

### `users`

| Column | Type | Notes |
|---|---|---|
| `id` | INTEGER PK | Auto-increment |
| `name` | TEXT | Required |
| `email` | TEXT | Unique · must be `@aiu.edu.eg` |
| `password_hash` | TEXT | bcrypt hash |
| `role` | TEXT | `student` · `admin` · `manager` |
| `university_id` | TEXT | Optional student ID |
| `created_at` | TEXT | Auto timestamp |

### `opportunities`

| Column | Type | Notes |
|---|---|---|
| `id` | INTEGER PK | Auto-increment |
| `title` | TEXT | Required |
| `category` | TEXT | e.g. `Internship` · `Dual Degree` · `Exchange Semester` |
| `field` | TEXT | e.g. `Computer Science` · `Business` |
| `location` | TEXT | City, Country |
| `provider` | TEXT | Partner university or company |
| `term` | TEXT | e.g. `Summer 2026` · `One Year` |
| `description` | TEXT | Full description |
| `requirements` | TEXT | Eligibility requirements |
| `status` | TEXT | `active` (default) or `inactive` |
| `created_at` | TEXT | Auto timestamp |

### `applications`

| Column | Type | Notes |
|---|---|---|
| `id` | INTEGER PK | Auto-increment |
| `user_id` | INTEGER FK | References `users.id` — CASCADE delete |
| `opportunity_id` | INTEGER FK | References `opportunities.id` — CASCADE delete |
| `statement` | TEXT | Personal statement |
| `recommendation_professor` | TEXT | Professor name |
| `graduation_year` | TEXT | Expected graduation |
| `status` | TEXT | `submitted` (default) |
| `created_at` | TEXT | Auto timestamp |
| UNIQUE | — | `(user_id, opportunity_id)` — one application per opportunity |

---

## 🔒 Auth Notes

Authentication uses a **simple header-based approach** (`x-user-role`, `x-user-id`) suitable for this university project. For production, replace with **JWT tokens** or server-side sessions.

---

## 📦 Available Scripts

```bash
npm start      # Start with Node
npm run dev    # Start with nodemon (hot reload)
```

---

## 📄 License

MIT © 2025 — AIU Global Hub · Built as a Web Development course project (Phase 2)
