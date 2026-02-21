# Tenant Isolation Rules

Movith Forms is a strict multi-tenant SaaS.

MANDATORY:

- Every tenant-scoped entity must include TenantId.
- All queries must include tenant filtering.
- Global query filters must be configured in DbContext.
- Background jobs must receive TenantId context.
- Dashboard aggregations must filter by TenantId.

FORBIDDEN:

- Raw SQL without tenant constraint
- Global DbSet access without filter
- Cross-tenant aggregation
- Caching without tenant scoping

If any implementation violates tenant isolation,
the agent must refuse.
