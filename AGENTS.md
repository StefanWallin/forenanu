# AGENTS.md

## Project

**Förena** (förena.nu) — Swedish for "Unite"  
Non-profit organization administration tool with public GraphQL API.

This repo is the **OSS self-hostable core**. A separate cloud repo handles multi-tenant SaaS infrastructure (Odoo billing, FluxCD, marketing site).

## Stack

| Layer | Technology |
|-------|------------|
| Monorepo | pnpm workspaces (all in `packages/`) |
| Language | TypeScript (strict) |
| Linting | Biome (format + lint) |
| API | GraphQL Yoga + Pothos (code-first schema) |
| Web | Next.js |
| Database | PostgreSQL + Drizzle ORM |
| Cache | Redis |
| Queue | RabbitMQ |
| Search/APM | OpenSearch + OpenTelemetry |
| Storage | S3-compatible (MinIO in dev) |
| Images | imgproxy |
| Auth | OIDC-agnostic (Keycloak bundled in dev) |
| Testing | Vitest (unit/integration) + Playwright (E2E) |
| i18n | English + Swedish, community translation ready |

## Architecture

- **Database-per-tenant, tenant-per-deployment**: each tenant gets its own DB and API deployment
- **Event-driven**: writes publish to RabbitMQ → consumers handle notifications, search indexing
- **RBAC**: DB-stored roles with optional IdP group mapping rules
- **Moderation**: local small LLM service (optional external API fallback)
- **Content**: GitHub Flavored Markdown with git-style audit trail (commit messages, diffs, full history publicly visible)

## Package Structure

```
packages/
├── api/           # @forena/api - GraphQL API server
├── web-public/    # @forena/web-public - Public customer website
├── web-member/    # @forena/web-member - Member portal
├── schema/        # @forena/schema - Shared TypeScript types
├── db/            # @forena/db - Drizzle schema + migrations
└── ui/            # @forena/ui - Shared React components
```

## Commands

```bash
# Development
docker-compose up -d          # Start infra (Keycloak, PostgreSQL, Redis, RabbitMQ, MinIO, OpenSearch, imgproxy)
pnpm install                  # Install dependencies
pnpm -w dev                   # Run all packages in dev mode

# Single package
pnpm --filter @forena/api dev
pnpm --filter @forena/api test

# All packages
pnpm -w build                 # Build all
pnpm -w test                  # Test all
pnpm -w lint                  # Lint (Biome)
pnpm -w typecheck             # TypeScript check
```

## Environment Variables

Core services (bring your own connection strings for self-hosting):

```bash
DATABASE_URL=           # PostgreSQL
REDIS_URL=              # Redis
RABBITMQ_URL=           # RabbitMQ
OPENSEARCH_URL=         # OpenSearch

# Storage
S3_ENDPOINT=
S3_BUCKET=
S3_ACCESS_KEY=
S3_SECRET_KEY=

# Auth
OIDC_ISSUER_URL=        # Keycloak or any OIDC provider

# Notifications (optional)
SMTP_HOST=
SMTP_USER=
SMTP_PASS=
TWILIO_ACCOUNT_SID=     # or 46ELKS_*
TWILIO_AUTH_TOKEN=
```

## Key Conventions

- **GraphQL versioning**: no explicit versions — use field deprecation + additive changes
- **Content editing**: all posts/pages/comments use GHFMD with optional raw markdown editing
- **Audit trail**: every edit has a single-line commit message, full diff history, publicly visible
- **Moderated content**: appears saved to user but triggers report to admin + ICE contact with diff
- **Events**: full system with recurring templates, SEO metadata, multi-channel notifications (email/SMS)
- **Members**: core fields + org-defined custom fields with customizable profile templates
- **Groups**: hierarchical (board → committees → working groups → chapters)
- **Notifications**: SMTP for email, easy setup for Twilio or 46elks for SMS

## Self-Hosting

1. Clone repo
2. Copy `.env.example` to `.env`, fill in connection strings
3. `docker-compose up -d` (or bring your own PostgreSQL/Redis/RabbitMQ/OpenSearch/MinIO)
4. `pnpm install && pnpm -w build`
5. Run containers or deploy to Kubernetes

Single-tenant deployment: one database, one deployment, configure your OIDC provider.
