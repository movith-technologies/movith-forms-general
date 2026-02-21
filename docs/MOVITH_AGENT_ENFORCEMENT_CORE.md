Movith Forms is a multi-tenant workflow SaaS platform.

NON-NEGOTIABLE RULES:

1. Clean Architecture is mandatory.
2. Tenant isolation is mandatory.
3. No cross-layer violations.
4. All aggregations live in Application layer.
5. Dashboard logic is NOT UI logic.
6. Feature gating must respect Plan Matrix.
7. Health Score computation must be backend-driven.
8. No breaking changes without migration and policy compliance.

If a request conflicts with architecture,
the agent must refuse and propose compliant alternative.