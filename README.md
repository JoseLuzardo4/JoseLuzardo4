# Hi there, I'm José Luzardo 👋

### Junior Software Engineer | Full-Stack | Python · FastAPI · React · PostgreSQL

Mechatronics Engineer transitioning into Software Engineering. My background bridges systems thinking with modern software architecture — an unusual combination that lets me reason about constraints, performance, and system design from first principles.

Currently building a compliance platform for Fintech and completing CS50x (Harvard) to formalize my Computer Science foundations.

📍 Ecuador (UTC-5) — Full overlap with US East Coast. Open to remote roles worldwide.

---

## 🛠️ Tech Stack

### Backend
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)

### Frontend
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)

### Database & Infrastructure
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

---

## 🔒 Selected Projects

> ⚠️ Source code for client projects is private due to NDAs. Architecture and technical decisions are documented below.

---

### 💸 Veritics — AML Compliance SaaS (Fintech, Active Development)

Real-time Anti-Money Laundering screening platform verifying transactions against international sanctions lists (OFAC, Interpol, UN).

```
┌─────────────────────────────────────────────────────────────┐
│                     VERITICS ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   React SPA          FastAPI Backend        PostgreSQL       │
│  ┌─────────┐        ┌─────────────┐        ┌──────────┐    │
│  │Auditor  │        │             │        │          │    │
│  │  View   │◄──────►│  REST API   │◄──────►│  RLS     │    │
│  ├─────────┤  JWT   │             │        │Policies  │    │
│  │Operator │        │  Auth       │        │          │    │
│  │  View   │        │  Middleware │        │Multi     │    │
│  └─────────┘        │             │        │Tenant    │    │
│                     │  AML Engine │        │Isolation │    │
│                     └─────────────┘        └──────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Tenant Isolation | PostgreSQL Row-Level Security (RLS) | Enforces isolation at DB layer, not just application layer |
| Authentication | JWT stateless sessions | Scales horizontally without shared session state |
| SQL Injection | Parameterized queries throughout | Eliminates injection vectors at the driver level |
| Role Separation | RBAC at API + UI level | Auditors and Operators see different views of the same data |

**Stack:** Python · FastAPI · PostgreSQL · React · Docker · JWT

---

### 🚛 Logistics Route Optimization — Tía S.A. (Past Work)

Delivery route optimization for a large Ecuadorian retail chain operating a heterogeneous fleet across 200+ daily deliveries with complex site-access constraints.

```
┌─────────────────────────────────────────────────────────────┐
│                  ASYNC SOLVER ARCHITECTURE                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Dispatcher         FastAPI          Redis         Celery    │
│  ┌────────┐        ┌───────┐        ┌─────┐      ┌──────┐  │
│  │        │ POST   │       │ Task   │     │Async │      │  │
│  │ Client │───────►│  API  │───────►│Queue│─────►│Worker│  │
│  │        │        │       │        │     │      │      │  │
│  │        │◄───────│202    │        └─────┘      │OR-   │  │
│  │        │Accepted│       │                     │Tools │  │
│  │        │        └───────┘                     │Solver│  │
│  │        │                                      └──┬───┘  │
│  │        │◄─────────────────Webhook + Routes───────┘      │
│  └────────┘                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**The Problem:** Split Delivery VRP (SDVRP) with Time Windows — an NP-hard combinatorial optimization problem. The solver needed to decide whether sending 2x Medium trucks ($140) was more cost-effective than 1x Large + 1x Small ($145) factoring in distance, fleet availability, and store access restrictions.

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Solver | Google OR-Tools Constraint Programming | Handles multi-dimensional cost functions natively |
| Async Decoupling | FastAPI → Redis → Celery | Prevents HTTP 504 timeouts on 30s+ calculations |
| Deployment | Docker containerization | Consistent cold-start performance across environments |

**Business Impact:** ~$3,000/month reduction in operational transport costs through optimized fleet mixing.

**Stack:** Python · FastAPI · Google OR-Tools · Redis · Celery · Docker · PostgreSQL

---

### 🧾 FacPlus — Electronic Invoicing SaaS (Production)

Multi-tenant SaaS for electronic invoicing, built during internship and currently serving ~300 client accounts in production.

```
┌─────────────────────────────────────────────────────────────┐
│                   FACPLUS ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Web (Laravel)    Python Microservice      Flutter Mobile    │
│  ┌────────────┐   ┌────────────────┐      ┌─────────────┐  │
│  │            │   │                │      │             │  │
│  │  Laravel   │──►│  Crypto Bridge │      │  Flutter    │  │
│  │  Core      │   │                │      │  Mobile App │  │
│  │            │   │  .p12 Legacy   │      │             │  │
│  │  Multi     │   │  Decryption    │      │  REST API   │  │
│  │  Tenant    │   │  (OpenSSL 1.x) │      │  Sync       │  │
│  │  RBAC      │   │                │      │             │  │
│  └─────┬──────┘   └────────────────┘      └─────────────┘  │
│        │                                                     │
│        ▼                                                     │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              PostgreSQL (Tenant-Scoped)               │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**The Engineering Challenge:** Ubuntu 24.04 ships with OpenSSL 3.1, which deprecated algorithms required by the tax authority's legacy .p12 certificates. Downgrading the OS security posture was not an option.

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Crypto Isolation | Dedicated Python microservice | Sandboxes legacy OpenSSL dependencies away from core |
| Tenant Isolation | Database-level query scoping | Zero data bleed between client accounts |
| Access Control | Hierarchical RBAC | Super Admin → Admin → Operator permission inheritance |

**Stack:** PHP · Laravel · Python · PostgreSQL · Flutter · Docker

---

## 📚 Currently Learning

```
CS50x Progress (Harvard/edX)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Week 1  ████████████████████  C, Memory, Algorithms
Week 2  ████████████████████  Arrays, Strings, Cryptography
Week 3  ████████████████████  Algorithms, Sorting, Graph Theory
Week 4  ████████████████████  Memory, Pointers, File I/O
Week 5  ████████████████████  Data Structures, Hash Tables
Week 6  ████████████████████  Python
Week 7  ░░░░░░░░░░░░░░░░░░░░  SQL (In Progress)
Week 8  ░░░░░░░░░░░░░░░░░░░░  HTML, CSS, JavaScript
Week 9  ░░░░░░░░░░░░░░░░░░░░  Flask
Week 10 ░░░░░░░░░░░░░░░░░░░░  Final Project

Notable completions: Tideman (graph cycle detection via DFS),
Speller (hash table from scratch with valgrind-clean memory),
Filter (image convolution with Sobel operator)
```

**Target Certifications:**
- AWS Certified Cloud Practitioner (CLF-C02) — Q3 2026

---

## 🗺️ Roadmap

Technologies I'm actively working toward and will add to my stack as I build real proficiency:

| Technology | Status | Target |
|------------|--------|--------|
| Next.js | Planned | Q4 2026 |
| React Native | Planned | Q1 2027 |
| AWS (beyond CCP) | Planned | 2027 |
| Golang | Planned | 2027 |

---

## 📬 Connect

- **Email:** jose.luzardo.dev@gmail.com
- **LinkedIn:** [linkedin.com/in/jose-luzardo](https://linkedin.com/in/jose-luzardo)
- **Location:** Ecuador (UTC-5) — Available for full-stack and backend remote roles worldwide.
