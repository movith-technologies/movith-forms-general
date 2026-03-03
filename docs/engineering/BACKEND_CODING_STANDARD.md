# BACKEND CODING STANDARD
# Project: MOVITH FORMS SaaS
# Stack: .NET 9 + Clean Architecture + CQRS + MediatR

This document defines mandatory backend coding rules.
All AI agents must strictly comply.

Architecture stability > speed.

------------------------------------------------------------
1. GENERAL PRINCIPLES
------------------------------------------------------------

- Code must be readable before being clever.
- Prefer explicitness over magic.
- Single Responsibility is mandatory.
- No hidden side-effects.
- No premature optimization.
- Every feature must be SaaS-aware.
- All handlers must be MediatR-based
- No fat controllers
- DTOs must be explicit
- No AutoMapper magic beyond simple mapping
- Every entity modification requires migration

------------------------------------------------------------
2. NAMING CONVENTIONS
------------------------------------------------------------

Entities:
FormDefinition
WorkflowDefinition
Subscription
Tenant

Commands:
StartFormInstance
PerformWorkflowAction

Queries:
GetAllFormDefinitions
GetTenantDashboardQuery

Handlers:
StartFormInstanceCommandHandler

DTOs:
FormDefinitionDto
WorkflowDefinitionDto

Interfaces:
IFormDefinitionRepository
ISubscriptionService
IAIService

Private fields:
_camelCase

Public members:
PascalCase

Async methods:
Must end with "Async"

------------------------------------------------------------
3. CQRS RULES
------------------------------------------------------------

Commands:
- Modify state
- Return minimal data
- No heavy reads

Queries:
- Read-only
- Must not modify state
- Optimized for performance

Handlers must:
- Be small (<150 lines)
- Contain single use-case
- Avoid nested condition blocks
- Not call DbContext directly

------------------------------------------------------------
4. RESULT PATTERN
------------------------------------------------------------

Never return raw exceptions.

Always use:

Result
Result<T>

Bad:
throw new Exception("Invalid")

Good:
return Result.Failure("Form not found")

------------------------------------------------------------
5. VALIDATION
------------------------------------------------------------

- Use FluentValidation
- Validation must live in Application layer
- No validation in Controllers
- No manual if-null chains in Handlers

------------------------------------------------------------
6. DEPENDENCY RULES
------------------------------------------------------------

Domain:
- No external references
- No infrastructure dependencies

Application:
- Only depends on Domain
- Uses Interfaces
- No EF Core usage

Infrastructure:
- Implements interfaces
- Contains EF Core
- Contains OpenAI implementation

WebAPI:
- Thin controllers only
- No business logic

------------------------------------------------------------
7. MULTI-TENANCY ENFORCEMENT
------------------------------------------------------------

Every tenant-owned query must:

- Filter by TenantId
- Validate tenant ownership
- Prevent cross-tenant access

Never assume tenant from frontend.
Always resolve from context.

------------------------------------------------------------
8. ASYNC RULES
------------------------------------------------------------

- All DB operations must be async
- No .Result
- No .Wait()
- Always pass CancellationToken

------------------------------------------------------------
9. LOGGING RULES
------------------------------------------------------------

- Use structured logging
- No string concatenation logs
- Log important domain events
- Log AI usage separately

Bad:
_logger.LogInformation("Tenant " + tenantId + " created")

Good:
_logger.LogInformation("Tenant created {TenantId}", tenantId)

------------------------------------------------------------
10. EXCEPTION HANDLING
------------------------------------------------------------

- Do not use try/catch for flow control
- Global exception middleware handles unhandled errors
- Business errors must use Result pattern

------------------------------------------------------------
11. AI INTEGRATION RULES
------------------------------------------------------------

- Never call OpenAI directly in Handler
- Always use IAIService
- Log token usage
- Respect subscription plan
- AI failures must not crash system

------------------------------------------------------------
12. MAPPING RULES
------------------------------------------------------------

- Use explicit mapping
- Avoid heavy AutoMapper magic
- Mapping must live in Application

------------------------------------------------------------
13. PERFORMANCE RULES
------------------------------------------------------------

- Use projection for queries
- Do not load full entities when unnecessary
- Avoid N+1 queries
- Index tenant-based fields

------------------------------------------------------------
14. CODE SMELLS (FORBIDDEN)
------------------------------------------------------------

❌ God Handlers
❌ Fat Controllers
❌ Repository inside Controller
❌ DbContext inside Application
❌ 300+ line methods
❌ Static service access
❌ Hardcoded plan logic
❌ Cross-tenant shortcuts

------------------------------------------------------------
15. TESTABILITY RULE
------------------------------------------------------------

Handlers must be:

- Testable without Infrastructure
- Mockable via interfaces
- Deterministic

------------------------------------------------------------
16. DOCUMENTATION RULE
------------------------------------------------------------

Complex logic must include:

- Why explanation
- Not what explanation

Example:

// WHY:
// We prevent re-bidding after deadline
// because audit integrity must be preserved.

------------------------------------------------------------
17. ENTERPRISE SAFETY RULE
------------------------------------------------------------

All backend code must support:

- 1000+ tenants
- AI usage billing
- Subscription upgrades
- Audit logging
- Regulatory compliance extension

------------------------------------------------------------
EXCEPTION HANDLING
------------------------------------------------------------

Exception and error-handling rules are defined in a separate document.
Before adding or refactoring exceptions, read and apply:

  docs/engineering/BACKEND_EXCEPTION_HANDLING.md

Key rule: one exception type per HTTP status; all mapping in
CustomExceptionHandler; no try-catch in endpoints for status mapping.

------------------------------------------------------------
FINAL RULE
------------------------------------------------------------

If a solution breaks architecture discipline,
AI must refuse and propose a compliant alternative.

Consistency > Cleverness
Scalability > Shortcuts
Stability > Speed

END OF DOCUMENT
