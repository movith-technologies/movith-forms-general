# SYSTEM DESIGN – HIGH LEVEL
# Project: MOVITH FORMS SaaS
# Architecture: Clean Architecture + Multi-Tenant + AI-Enabled

This document describes the high-level system architecture,
major components, boundaries, and scalability direction.

------------------------------------------------------------
1. SYSTEM OVERVIEW
------------------------------------------------------------

MOVITH FORMS is a:

- Multi-tenant SaaS platform
- Subscription-based
- AI-augmented
- Enterprise-ready
- Cloud-native

Core pillars:

- Tenant Isolation
- Clean Architecture
- Feature-Based Modularity
- AI Monetization
- Observability
- Scalability

------------------------------------------------------------
2. HIGH-LEVEL COMPONENTS
------------------------------------------------------------

[ Client Layer ]
    - Next.js Frontend
    - Tenant-aware UI
    - Role & Plan gated features

            ↓

[ API Layer ]
    - ASP.NET Core Web API
    - Authentication & Authorization
    - Middleware (Tenant resolution)
    - Rate limiting

            ↓

[ Application Layer ]
    - Use Cases
    - CQRS (Commands & Queries)
    - Validation
    - Business rules
    - Plan enforcement
    - AI usage enforcement

            ↓

[ Domain Layer ]
    - Entities
    - Aggregates
    - Domain rules
    - Subscription logic
    - Tenant ownership rules

            ↓

[ Infrastructure Layer ]
    - EF Core (Database)
    - AI Provider abstraction
    - File storage
    - Email
    - External integrations

------------------------------------------------------------
3. CLEAN ARCHITECTURE BOUNDARIES
------------------------------------------------------------

Dependency rule:

Domain ← Application ← Infrastructure ← Web

Domain has zero external dependency.

Application depends only on abstractions.

Infrastructure implements interfaces.

Web only orchestrates.

------------------------------------------------------------
4. MULTI-TENANT MODEL
------------------------------------------------------------

Primary strategy:

Shared database
TenantId per entity
Global query filters

Tenant resolved from:

- Authenticated user claims
- Middleware
- Secure context

Never from request payload.

------------------------------------------------------------
5. AUTHENTICATION FLOW
------------------------------------------------------------

User Login
    ↓
Token issued (contains TenantId)
    ↓
Secure HttpOnly cookie
    ↓
Request pipeline resolves tenant
    ↓
Application layer enforces tenant boundary

------------------------------------------------------------
6. SUBSCRIPTION & FEATURE GATING
------------------------------------------------------------

Each Tenant has:

- Subscription plan
- Plan features
- AI token quota
- Limits

Before executing:

- Validate feature access
- Validate plan limits
- Validate quota

Frontend hides features
Backend enforces rules

------------------------------------------------------------
7. AI ARCHITECTURE POSITIONING
------------------------------------------------------------

AI is:

- A bounded subsystem
- Monetized
- Tracked
- Provider-abstracted

Flow:

User → API → Application (IAIService)
        ↓
    Plan validation
        ↓
    Token estimation
        ↓
    Infrastructure (IAIProvider)
        ↓
    External AI (OpenAI / Azure / etc.)

All calls logged in AIUsageLogs.

------------------------------------------------------------
8. DATA STORAGE
------------------------------------------------------------

Primary DB:
PostgreSQL or SQL Server

Tables include:

- Tenants
- Users
- Roles
- Subscriptions
- Plans
- FormDefinitions
- WorkflowDefinitions
- AIUsageLogs

All tenant-owned tables contain TenantId.

Indexes:

- TenantId
- Foreign keys
- Frequently filtered fields

------------------------------------------------------------
9. FILE STORAGE
------------------------------------------------------------

Structure:

/storage/{tenantId}/{feature}/{fileId}

Files must:

- Require authentication
- Be tenant-isolated
- Not be publicly accessible

Future:

- Object storage (S3 compatible)
- Tenant-based buckets (enterprise tier)

------------------------------------------------------------
10. OBSERVABILITY & LOGGING
------------------------------------------------------------

System must log:

- Requests
- Errors
- Cross-tenant attempts
- AI usage
- Subscription limit breaches

Must include:

TenantId
UserId
CorrelationId
Timestamp

Future:

- Centralized logging
- Metrics dashboard
- Alert system

------------------------------------------------------------
11. SCALABILITY STRATEGY
------------------------------------------------------------

Phase 1:
Monolithic deployment (modular internally)

Phase 2:
Split into services if needed:

- AI Service
- Billing Service
- Notification Service

Phase 3:
Horizontal scaling
Load balancing
Background workers
Queue-based AI jobs

System must be designed
to allow gradual decomposition.

------------------------------------------------------------
12. PERFORMANCE STRATEGY
------------------------------------------------------------

Backend:

- Indexing
- Query optimization
- Pagination mandatory
- Avoid N+1 queries

Frontend:

- Server Components by default
- Dynamic imports
- React Query caching
- Minimal client components

------------------------------------------------------------
13. SECURITY MODEL
------------------------------------------------------------

Layers:

1) Authentication
2) Authorization
3) Tenant isolation
4) Plan enforcement
5) Rate limiting
6) Audit logging

Security is layered, not single-point.

------------------------------------------------------------
14. DEPLOYMENT MODEL
------------------------------------------------------------

Cloud-ready containerized deployment:

- Dockerized backend
- Dockerized frontend
- Reverse proxy (Nginx)
- HTTPS mandatory

CI/CD:

- Build
- Test
- Migrate DB
- Deploy
- Health check

------------------------------------------------------------
15. FUTURE EXTENSIONS
------------------------------------------------------------

Architecture must support:

- AI Agents (multi-step workflows)
- Real-time features (WebSocket)
- Marketplace modules
- Enterprise isolated DB per tenant
- Microservice extraction

No architectural decision should block these.

------------------------------------------------------------
16. SYSTEM PRINCIPLES
------------------------------------------------------------

- Tenant boundary is sacred
- Backend is source of truth
- AI is monetized
- Plans control features
- Observability is mandatory
- Scalability is incremental
- Security > Speed
- Stability > Trendiness

------------------------------------------------------------
17. DIAGRAM SUMMARY (MENTAL MODEL)
------------------------------------------------------------

User
  ↓
Frontend (Next.js)
  ↓
Web API
  ↓
Application (CQRS + Policies)
  ↓
Domain (Rules)
  ↓
Infrastructure
     ├── Database
     ├── AI Provider
     ├── File Storage
     └── External Services

All guarded by:
Tenant Context + Plan Enforcement + Logging

------------------------------------------------------------
FINAL STATEMENT
------------------------------------------------------------

MOVITH FORMS is not a CRUD app.

It is a:

- Multi-tenant SaaS platform
- AI-enabled revenue system
- Enterprise-grade architecture
- Scalable cloud-native solution

Design decisions must always protect:

Isolation
Monetization
Security
Extensibility

END OF DOCUMENT
