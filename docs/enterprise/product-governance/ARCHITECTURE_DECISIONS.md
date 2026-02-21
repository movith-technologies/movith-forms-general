# Architecture Decisions (ADR)
Movith Forms – Multi-Tenant Workflow SaaS

This document records major architectural decisions.
All contributors and AI agents must respect these decisions.

If a proposal conflicts with any decision here,
the agent must STOP and propose a compliant alternative.

---

## ADR-001: Clean Architecture is Mandatory

Decision:
Movith Forms uses strict Clean Architecture.

Layers:
- Domain
- Application
- Infrastructure
- Web

Rationale:
- Maintain long-term scalability
- Prevent tight coupling
- Enable testability
- Support enterprise growth

Consequences:
- No cross-layer references
- No business logic in Web
- No Infrastructure access from UI
- All use cases implemented via Application layer

Status: Accepted
Non-negotiable.

---

## ADR-002: Multi-Tenant Logical Isolation

Decision:
System operates as a logically isolated multi-tenant SaaS.

Rationale:
- SaaS scalability
- Security compliance
- Enterprise readiness

Rules:
- All tenant-scoped entities must include TenantId
- All queries must enforce TenantId filtering
- No cross-tenant aggregation
- Background jobs must be tenant-aware

Consequences:
- Global DbSet access without filtering is forbidden
- Raw SQL must include tenant constraint

Status: Accepted
Non-negotiable.

---

## ADR-003: Backend-Driven Business Logic

Decision:
All business logic must live in Application or Domain layers.

Rationale:
- Prevent UI logic drift
- Ensure consistency
- Enable reuse (API, background jobs, integrations)

Forbidden:
- SLA calculation in frontend
- Health Score calculation in UI
- Workflow state transitions from controllers

Status: Accepted

---

## ADR-004: DTO-Only External Contracts

Decision:
Domain Entities must never be exposed outside Application layer.

Rationale:
- Protect domain integrity
- Prevent accidental mutation
- Maintain contract stability

Rules:
- All APIs return DTOs
- Mapping must be explicit
- No Entity serialization

Status: Accepted

---

## ADR-005: Health Score is Deterministic & Backend-Computed

Decision:
System health metrics are computed in Application layer.

Health Score inputs:
- Completion Rate
- SLA Compliance
- Error Rate
- Submission Trend
- Drop-off Rate

Rationale:
- Executive-level decision support
- Consistent scoring across clients
- Prevent UI drift

Consequences:
- UI may only render score
- Formula must be versioned
- Changes require ADR update

Status: Accepted

---

## ADR-006: Subscription & Feature Gating Architecture

Decision:
Feature gating must be dynamic and plan-based.

Rationale:
- SaaS monetization
- Enterprise tier differentiation
- Future scalability

Rules:
- Plan logic lives in Application layer
- UI must not hardcode plan checks
- Feature access must be tenant-scoped
- Plan matrix stored in database

Status: Accepted

---

## ADR-007: Deterministic Workflow Engine

Decision:
Workflow engine must be deterministic.

Rationale:
- Auditability
- Compliance
- SLA enforcement
- Enterprise reliability

Rules:
- State transitions validated in Domain layer
- No AI-driven state mutation
- All transitions logged

Status: Accepted

---

## ADR-008: Observability is First-Class

Decision:
Observability is mandatory, not optional.

Must include:
- Structured logging
- Error tracking
- SLA breach logging
- Performance metrics per tenant

Rationale:
- Enterprise readiness
- Debugging
- Operational maturity

Status: Accepted

---

## ADR-009: Migration Discipline

Decision:
All schema changes require migration.

Rationale:
- Multi-tenant data safety
- Version traceability
- Deployment reliability

Rules:
- No silent schema changes
- No runtime schema mutation
- All changes documented

Status: Accepted

---

## ADR-010: AI is Optional, Never Core

Decision:
AI features are auxiliary and must never control core logic.

Rationale:
- Predictability
- Cost control
- Determinism
- Enterprise trust

Rules:
- AI cannot modify workflow state
- AI cannot bypass SLA
- AI must respect tenant isolation
- AI must be metered per tenant

Status: Accepted

---

## ADR Governance Rule

If any new feature:
- Breaks Clean Architecture
- Weakens tenant isolation
- Moves logic to UI
- Hardcodes subscription logic
- Introduces tight coupling

It must:
1. Be rejected, or
2. Require a new ADR with documented trade-offs.

Architecture stability is prioritized over rapid feature addition.

---

End of Document.