# ENTERPRISE MULTI-DB STRATEGY
# Project: MOVITH FORMS SaaS
# Purpose: Scalable Tenant Isolation & Enterprise-Grade Deployment Flexibility

This document defines:

- Multi-tenant DB architecture
- Isolation levels
- Enterprise database separation
- Scaling strategy

------------------------------------------------------------
1. CORE PRINCIPLE
------------------------------------------------------------

Not all tenants are equal.

Architecture must support:

Shared DB model (SaaS default)
Dedicated DB model (Enterprise)
Hybrid migration

------------------------------------------------------------
2. TENANT ISOLATION LEVELS
------------------------------------------------------------

Level 1:
Shared Database
Shared Schema
TenantId column isolation

Level 2:
Shared Database
Separate Schema per tenant

Level 3:
Dedicated Database per tenant

Enterprise must support Level 3.

------------------------------------------------------------
3. DEFAULT MODEL (PHASE 1)
------------------------------------------------------------

Shared DB
TenantId column
Strict query filters

All queries must include:

TenantId filter.

Global query filters required.

------------------------------------------------------------
4. ENTERPRISE MODEL
------------------------------------------------------------

Enterprise tenant may have:

Dedicated DB
Dedicated vector store
Dedicated cache
Custom connection string

Tenant must define:

IsolationMode

System must resolve:

Correct connection per request.

------------------------------------------------------------
5. DATABASE RESOLUTION FLOW
------------------------------------------------------------

Request arrives:

1) Extract TenantId
2) Fetch Tenant configuration
3) Resolve DB connection
4) Create scoped DbContext

Never:

Use static DB connection.

------------------------------------------------------------
6. MIGRATION STRATEGY
------------------------------------------------------------

Must support:

Promoting tenant from shared DB
to dedicated DB.

Steps:

1) Freeze tenant writes
2) Copy tenant data
3) Validate consistency
4) Switch connection
5) Resume operations

------------------------------------------------------------
7. PERFORMANCE STRATEGY
------------------------------------------------------------

Monitor:

- Per-tenant DB usage
- Heavy query tenants
- Large storage consumers

High-usage tenants may be:

Upgraded to dedicated DB.

------------------------------------------------------------
8. SECURITY ENFORCEMENT
------------------------------------------------------------

Even with dedicated DB:

Application layer tenant validation remains mandatory.

Never rely only on DB isolation.

------------------------------------------------------------
9. BACKUP STRATEGY
------------------------------------------------------------

Shared DB:
Full backup

Dedicated DB:
Per-tenant backup

Enterprise may request:

Custom backup retention

------------------------------------------------------------
10. COST STRATEGY
------------------------------------------------------------

Dedicated DB increases cost.

Enterprise plan must reflect:

- Infra cost
- Maintenance cost
- Support cost

Architecture must track:

Infra cost per enterprise tenant.

------------------------------------------------------------
11. SCALING STRATEGY
------------------------------------------------------------

Allow:

Read replicas
Sharding (future)
Geo deployment

Design must not assume single DB.

------------------------------------------------------------
12. VECTOR DB ALIGNMENT
------------------------------------------------------------

If tenant has:

Dedicated DB

They may also have:

Dedicated vector namespace
Or dedicated vector cluster

------------------------------------------------------------
13. TENANT CONFIGURATION MODEL
------------------------------------------------------------

Tenant entity must include:

IsolationMode
DatabaseConnectionString
VectorNamespace
CustomSettings

------------------------------------------------------------
14. FORBIDDEN PRACTICES
------------------------------------------------------------

❌ Static connection string
❌ Ignoring tenant isolation in queries
❌ Hardcoding enterprise overrides
❌ Manual DB switching
❌ Schema drift between tenants

------------------------------------------------------------
FINAL PRINCIPLE
------------------------------------------------------------

Multi-tenant SaaS must be:

Flexible
Isolatable
Upgradable

Enterprise readiness is not feature.
It is architectural posture.

END OF DOCUMENT
