# Hi there, I'm José Luzardo 👋

### Full-Stack Software Engineer | React · Python · Go · FastAPI · PostgreSQL

Mechatronics Engineer turned Full-Stack Software Engineer. My background bridges systems thinking — control loops, tolerances, failure modes — with modern web architecture. I like owning products end to end: the interface someone actually clicks on, and the backend that keeps its numbers honest under load.

Recently completed **CS50x (Harvard)** and currently working as a software engineer for a fintech startup, building both the AML compliance dashboard and the FastAPI backend behind it.

📍 Ecuador (UTC-5) — Full overlap with US East Coast. Open to remote roles worldwide.

---

## 🛠️ Tech Stack

### Frontend
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

### Backend
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Laravel](https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)

### Database & Infrastructure
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

---

## 💼 Currently

- 🔐 Building the MVP of a regulatory AML/CFT compliance platform for a fintech startup — an AHP-based risk-scoring engine, multi-tenant RLS, and a React dashboard, designed to withstand direct regulatory audit.
- 🎓 Fresh off **CS50x**, applying those fundamentals (memory management, algorithmic complexity, hash tables) directly to production systems.
- 📦 Working toward **AWS Certified Cloud Practitioner (CLF-C02)**.

---

## 🔒 Selected Projects

> ⚠️ Source code for client projects is private due to NDAs. Architecture and technical decisions are documented below. The CS50x final project is public — see the featured project first.

---

### 🎓 BNPL Core Engine & Dashboard — CS50x Final Project (Public)

A full-stack "Buy Now, Pay Later" platform built as my final project for Harvard's CS50x. A React dashboard sits on top of a Go REST API that simulates credit approval, splits purchases into 4 interest-free installments, and runs automated bank reconciliation — with the accounting rigor of a real ledger system.

```
┌─────────────────────────────────────────────────────────────┐
│                     BNPL ARCHITECTURE                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   React SPA           Go REST API          SQLite/Postgres   │
│  ┌─────────┐        ┌─────────────┐        ┌──────────┐    │
│  │Client   │        │ Scheduler   │        │  ACID    │    │
│  │Dashboard│◄──────►│ Service     │◄──────►│  Ledger  │    │
│  ├─────────┤  JWT   │             │        │          │    │
│  │Admin    │        │ Mutex +     │        │ Integer  │    │
│  │Console  │        │ Row Locking │        │ Cents    │    │
│  └─────────┘        └─────────────┘        └──────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Currency Precision | All amounts stored as integer cents | Eliminates float rounding errors in a real ledger |
| Double-Spend Prevention | Two-tier locking: `sync.Mutex` (keyed by user) + DB row locking (`SELECT FOR UPDATE`) | Guards against race conditions even if the app-level lock is bypassed |
| Transaction Safety | Explicit `BEGIN...COMMIT` blocks around order + installment creation | All-or-nothing writes — no orphaned installments |
| Reconciliation | Async CSV parsing in a background goroutine, classified as Matched / Partial / Discrepancy | Non-blocking bank report ingestion at scale |

**Stack:** Go · React · PostgreSQL/SQLite · Docker · JWT · bcrypt

**Repo:** `github.com/joseluzardo-dev` *(link to be added)*

---

### 💸 Veritics — AML Compliance SaaS (Fintech, Active)

Regulatory AML/CFT compliance SaaS for Ecuador's non-financial obligated entities (notaries, real estate, law firms) — built to withstand direct audit scrutiny from Ecuador's financial intelligence unit (UAFE). I'm the engineer building the MVP, implementing a system architecture designed by the founding team together with a senior AML regulatory consultant.

```
┌─────────────────────────────────────────────────────────────┐
│                     VERITICS ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   React SPA          FastAPI Backend        PostgreSQL       │
│  ┌─────────┐        ┌─────────────┐        ┌──────────┐    │
│  │Admin    │        │ AHP Risk    │        │  RLS     │    │
│  │Operator │◄──────►│ Scoring     │◄──────►│Policies  │    │
│  │Auditor  │  JWT   │ Engine      │        │          │    │
│  │  Views  │        │ (3 Pillars) │        │Multi     │    │
│  └─────────┘        │ RBAC (4-tier)│       │Tenant    │    │
│                     └─────────────┘        └──────────┘    │
│                             │                                │
│                     ┌───────┴────────┐                       │
│                     │ Background      │                      │
│                     │ Scraping Engine │                      │
│                     │ (18+ sources)   │                      │
│                     └────────────────┘                       │
└─────────────────────────────────────────────────────────────┘
```

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Risk Scoring | AHP (Analytic Hierarchy Process) engine, 3 isolated risk pillars (Clients, Employees, Vendors) | Mathematically defensible score for regulatory audits; prevents false positives from bleeding across unrelated risk domains |
| Automatic Triggers | Hard-coded bypass rules (e.g., PEP detection) that override the scoring formula | Regulation is inflexible on certain conditions — no weighted variable should be able to soften a mandatory compliance flag |
| Background Scraping | Async worker queries 18+ external sources independently, notifies on completion | Avoids blocking the user on 18+ simultaneous external dependencies; isolates failures per source |
| Tenant Isolation | PostgreSQL Row-Level Security (RLS) | Isolation is structural at the DB layer, not just enforced in application code |
| RBAC | 4-tier role hierarchy (Superadmin, Admin, Operator, Auditor) with route-level enforcement | Restricted roles get a 404 on config pages, not a 403 — they can't even detect the feature exists |
| Audit Evidence | Timestamped, watermarked screenshots per source consulted | Defensible evidence trail if the client is audited by the regulator |
| Immutable Records | Soft deletes only; every config change generates a signed record | Complies with 10-year data retention regulation; protects against disputed changes |

**Stack:** Python · FastAPI · React · PostgreSQL · Playwright · Hetzner · JWT

---

### 🚛 Logistics Route Optimization — Enterprise Retail Corporation

Delivery route optimization for a large Ecuadorian retail chain operating a heterogeneous fleet across 200+ daily deliveries with complex site-access constraints. Built both the dispatcher-facing client and the FastAPI backend behind it. *(Fixed-term contract, Oct 2025 – Apr 2026, completed.)*

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

**The Problem:** Split Delivery VRP (SDVRP) with Time Windows — an NP-hard combinatorial optimization problem. The solver had to decide whether 2x Medium trucks or 1x Large + 1x Small was more cost-effective, factoring in distance, fleet availability, and store access restrictions.

**Key Technical Decisions:**

| Decision | Implementation | Why |
|----------|---------------|-----|
| Solver | Google OR-Tools Constraint Programming | Handles multi-dimensional cost functions natively |
| Async Decoupling | FastAPI → Redis → Celery | Prevents HTTP 504 timeouts on 30s+ calculations |
| Deployment | Docker containerization | 70% reduction in cold-start times |
| Data Pipelines | Automated ETL ingestion | 40% reduction in logistics setup time |

**Business Impact:** ~$3,000/month reduction in operational transport costs through optimized fleet mixing.

**Stack:** Python · FastAPI · Google OR-Tools · Redis · Celery · Docker · PostgreSQL

---

### 🧾 FacPlus — Electronic Invoicing SaaS (Production)

Multi-tenant SaaS for electronic invoicing, built end to end from an internship into a paid contract role — Laravel web app, MySQL schema, and companion Flutter mobile app — now serving ~300 client accounts in production.

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
| Subscriptions | Custom lifecycle logic | Prorated upgrades/downgrades, automated expiration triggers |

**Stack:** PHP · Laravel · Python · PostgreSQL · Flutter · Docker

---

## 📚 Education & Certifications

- 🎓 **B.S. in Mechatronics Engineering** — ESPOL (EUR-ACE® Accredited), focus on Control Logic & IoT
- 🎓 **CS50x: Introduction to Computer Science** — Harvard University (via edX) — *Completed 2026*
- 📜 **IBM Backend Development** — Professional Certificate (Coursera)
- 📜 **Meta Front-End Developer** — Professional Certificate (Coursera)
- 📜 **Go Programming Specialization** (Coursera)
- 📜 **Meta React Specialization** (Coursera)
- 🎯 **Target:** AWS Certified Cloud Practitioner (CLF-C02) — Q3 2026

---

## 🗺️ Roadmap

Technologies I'm actively working toward and will add to my stack as I build real proficiency:

| Technology | Status | Target |
|------------|--------|--------|
| Next.js | Planned | Q4 2026 |
| React Native | Planned | Q1 2027 |
| AWS (beyond CCP) | Planned | 2027 |
| Kubernetes | Planned | 2027 |

---

## 📬 Connect

- **Email:** jose.luzardo.dev@gmail.com
- **LinkedIn:** [linkedin.com/in/jose-luzardo](https://linkedin.com/in/jose-luzardo)
- **Location:** Ecuador (UTC-5) — Available for full-stack and backend remote roles worldwide.
