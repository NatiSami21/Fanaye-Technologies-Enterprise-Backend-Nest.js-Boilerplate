# Fanaye Technologies Enterprise Backend Nest.js Boilerplate
> **Architecture Paradigm**: Hexagonal Architecture + Dual-Process Isolation + Enterprise IAM + Asynchronous Queues  
> **Author & Lead Architect**: Natinael Samuel (2026) | afritioalberts1216@gmail.com | +251904161978

---

## 1. System Invariants & Non-Negotiable Directives

All AI coding assistants (Antigravity, Cursor, Claude Code, Copilot) operating within this repository MUST strictly abide by these directives:

1. **Hexagonal Persistence (Ports & Adapters)**:
   - Domain models (`src/*/domain/*.ts`) must **NEVER** import `@prisma/client`.
   - Repository ports must be abstract TypeScript classes used as NestJS injection tokens.
   - All persistence data transformations occur exclusively in dedicated pure two-way mappers (`src/*/infrastructure/persistence/relational/mappers/*-prisma.mapper.ts`).
   - Domain entities must be pure TypeScript classes with zero ORM annotations.

2. **Dual-Process Isolation**:
   - `src/main.ts` handles the Express HTTP REST API and WebSocket gateway on port 4000.
   - `src/main-worker.ts` boots an independent NestJS application context dedicated to consuming BullMQ background job queues.
   - Never run heavy asynchronous processing directly within HTTP request handlers. Dispatch jobs via BullMQ.

3. **Redis Invariant**:
   - BullMQ requires blocking Redis commands. All queue connections and worker options MUST declare `maxRetriesPerRequest: null`.

4. **Security & IAM Defense-in-Depth**:
   - Passwords must be hashed using **Argon2id** (`memoryCost: 19456, timeCost: 2, parallelism: 1`).
   - Legacy bcrypt hashes (`$2a$`, `$2b$`) must automatically rehash to Argon2id upon successful user login.
   - All password changes and registrations must pass Have I Been Pwned (HIBP) k-anonymity breach verification (with network fail-open fallback).
   - Multi-device sessions are tracked using Node.js `AsyncLocalStorage` (`requestDeviceContext`), capturing `X-Device-Id` and `User-Agent`.

5. **Memo - Living Memory Governance**:
   - Workspace utilizes the **Memo - Living Memory** extension (`natinaelsamuel.memo-living-memory`).
   - All major architectural shifts must be logged in `memory/progress_log.md` and `memory/edit_log.md`.

6. **Git Commit Phrasing**:
   - **Strictly No Robot Prefixes**: Never use conventional commit prefixes (`feat:`, `chore:`, `fix:`, `docs:`, `refactor:`, `agent:`).
   - Write natural, concise human phrases describing what the change achieves (e.g., *"Add invoice relational slice with hexagonal persistence"*).

---

## 2. Core Agent Skills (.agents/skills/)

| Skill | Directory | Core Purpose |
| :--- | :--- | :--- |
| **3-create-domain-slice** | `.agents/skills/3-create-domain-slice/` | Scaffolds synchronized DDD vertical slices via Hygen |
| **4-hexagonal-persistence** | `.agents/skills/4-hexagonal-persistence/` | Decouples Prisma 7 from domain logic via abstract ports and pure mappers |
| **5-enterprise-iam-defense** | `.agents/skills/5-enterprise-iam-defense/` | Argon2id, HIBP k-anonymity, 2FA TOTP, WebAuthn Passkeys, session tracking |
| **6-async-bullmq-worker** | `.agents/skills/6-async-bullmq-worker/` | Standalone BullMQ WorkerHost, Redis resilience (`maxRetriesPerRequest: null`) |
| **7-safe-db-migration** | `.agents/skills/7-safe-db-migration/` | Non-destructive schema evolution, accidental data-loss prevention |

---

## 3. Command Cheat Sheet

```bash
# Start infrastructure
docker compose up -d

# Development server
npm run dev

# Background queue worker
npm run start:worker:dev

# Prisma operations
npm run prisma:generate
npm run prisma:migrate
npm run prisma:seed
npm run prisma:studio

# Scaffold new hexagonal domain slice
npm run generate:resource

# Type safety check
npm run typecheck
```
