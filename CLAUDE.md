# Claude Code Guidance - Fanaye Technologies Enterprise Backend Nest.js Boilerplate
- Author: Natinael Samuel (2026) <afritioalberts1216@gmail.com>
- Architecture: NestJS 11, Prisma 7 with PostgreSQL 16 adapter, BullMQ with Redis 7, Argon2id IAM.
- Hexagonal Persistence: Domain models never import @prisma/client. Use abstract repository ports and pure mappers.
- Dual-Process: main.ts for HTTP/WebSockets, main-worker.ts for BullMQ background workers.
- Living Memory: Use Memo - Living Memory (natinaelsamuel.memo-living-memory) and keep memory/ updated.
- Commits: Use human, natural phrases. Never use robot commit prefixes (feat:, chore:, etc.).
