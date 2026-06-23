# Hi there, I'm José Luzardo 👋

### Junior Software Engineer | Full-Stack | Python · FastAPI · React · PostgreSQL

I'm a Mechatronics Engineer transitioning into Software Engineering, focused on building production-grade systems that solve real business problems. My background bridges hardware systems thinking with modern software architecture — a combination that gives me an unusual ability to reason about constraints, performance, and systems design from first principles.

Currently building a compliance platform for Fintech and completing CS50x (Harvard) to formalize my Computer Science foundations.

Open to remote roles worldwide (UTC-5, full overlap with US East Coast).

---

## 🛠️ Tech Stack

| Backend | Frontend | Database & Infra |
| :--- | :--- | :--- |
| ![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white) | ![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB) | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white) |
| ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white) | ![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black) | ![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white) |
| ![C](https://img.shields.io/badge/C-00599C?style=for-the-badge&logo=c&logoColor=white) | | ![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white) |

---

## 🔒 Selected Projects

> ⚠️ Source code for client projects is private due to NDAs. Architecture and technical decisions are documented below.

---

### 💸 Veritics — AML Compliance SaaS (Fintech, Active Development)

**Context:** End-to-end Anti-Money Laundering screening platform for a financial services client. Verifies transactions in real time against international sanctions lists (OFAC, Interpol, UN).

**Technical Decisions:**

- **Multi-tenant Architecture:** Designed strict tenant isolation using PostgreSQL Row-Level Security (RLS) policies, ensuring zero data bleed between clients at the database layer — not just at the application layer.
- **Security Hardening:** Implemented parameterized queries throughout to eliminate SQL injection vectors. Authentication built on JWT with stateless session management.
- **Backend:** FastAPI chosen for its async-first architecture and automatic OpenAPI documentation, which simplifies compliance audit requirements.
- **Frontend:** React SPA with role-based UI rendering — Auditors and Operators see different views of the same data based on their permissions.

**Stack:** Python · FastAPI · PostgreSQL · React · Docker · JWT

---

### 🚛 Logistics Route Optimization — Tía S.A. (Past Work)

**Context:** Delivery route optimization for a large retail chain operating a heterogeneous fleet of 200+ vehicles daily across multiple store locations with site-access restrictions.

**The Engineering Challenge:**
The problem was a Split Delivery Vehicle Routing Problem (SDVRP) with Time Windows — a class of NP-hard combinatorial optimization. Manual routing failed to account for fleet mixing tradeoffs (e.g., whether sending 2x Medium trucks at $140 was more cost-effective than 1x Large + 1x Small at $145 given distance and store constraints).

**Technical Decisions:**

- **Solver:** Google OR-Tools Constraint Programming model with a multi-dimensional cost function balancing distance, fixed fleet costs, and time window penalties.
- **Async Architecture:** Decoupled the CPU-intensive solver from the API using FastAPI → Redis → Celery to prevent HTTP timeouts on calculations exceeding 30 seconds.
- **Deployment:** Containerized solver workers with Docker for consistent cold-start performance.

**Business Impact:**
- 📉 ~$3,000/month reduction in operational transport costs through optimized fleet mixing.
- 🚀 Significant reduction in solver cold-start times through containerization.

**Stack:** Python · FastAPI · Google OR-Tools · Redis · Celery · Docker · PostgreSQL

---

### 🧾 FacPlus — Electronic Invoicing SaaS (Production)

**Context:** Multi-tenant SaaS platform for electronic invoicing, built during an internship and currently in production serving ~300 client accounts.

**The Engineering Challenge:**
The local tax authority (SRI) required digital signatures using legacy .p12 certificates with cryptographic algorithms deprecated in modern OpenSSL 3.x. The platform needed to handle this without downgrading the OS security posture.

**Technical Decisions:**

- **Cryptographic Microservice:** Isolated the legacy decryption logic into a dedicated Python microservice using low-level cryptography libraries, acting as a bridge between modern infrastructure and legacy certificate formats.
- **Multi-tenant Isolation:** Implemented tenant scoping at the database query level to ensure strict data isolation between client accounts.
- **Access Control:** Role-based system with hierarchical permissions (Super Admin → Admin → Operator).

**Stack:** Python · PHP/Laravel · PostgreSQL · Flutter (mobile) · Docker

---

## 📚 Currently Learning

- **CS50x (Harvard/edX):** Formalizing Computer Science fundamentals — algorithms, data structures, memory management, and systems programming in C and Python. Completed: Weeks 1–6 including Tideman (graph cycle detection), Speller (hash table implementation), and Filter (image convolution algorithms).
- **JavaScript & React:** Building toward full-stack proficiency in modern web development.
- **AWS Cloud Practitioner (CLF-C02):** Targeting certification Q3 2026.

---

## 📬 Connect

- **Email:** jose.luzardo.dev@gmail.com
- **LinkedIn:** [linkedin.com/in/jose-luzardo](https://linkedin.com/in/jose-luzardo)
- **Location:** Ecuador (UTC-5) — Available for full-stack and backend remote roles worldwide.
