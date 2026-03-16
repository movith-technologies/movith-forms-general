# GOLDEN FOLDER STRUCTURE BLUEPRINT
# Project: MOVITH FORMS SaaS
# Architecture: Clean Architecture + CQRS
# Type: Multi-Tenant B2B SaaS

This document defines the ONLY allowed folder structure.
AI agents must strictly follow this structure.

No custom folder improvisation is allowed.

------------------------------------------------------------
BACKEND STRUCTURE (.NET)
------------------------------------------------------------

src/
 ├── Domain/
 │     ├── Common/
 │     ├── Constants/
 │     ├── Entities/
 │     ├── Enums/
 │     ├── Events/
 │     └── Exceptions/
 │
 ├── Application/
 │     ├── Common/
 │     │     ├── Interfaces/
 │     │     ├── Behaviors/
 │     │     └── Models/
 │     │
 │     ├── Commands/
 │     │     ├── FormDefinitions/
 │     │     ├── Workflows/
 │     │     │
 │     │     │
 │     ├── Queries/
 │     │     ├── FormDefinitions/
 │     │     ├── Workflows/
 │     │     ├── Payments/
 │     │     └── Dashboard/
 │     │
 │     └── DependencyInjection.cs
 │
 ├── Infrastructure/
 │     ├── Persistence/
 │     │     ├── Configurations/
 │     │     └── ApplicationDbContext.cs
 │     │
 │     ├── Repositories/
 │     │     └── BiReportRepository.cs  ← Lives here, NOT under Persistence/
 │     │
 │     ├── Migrations/                     ← Lives here, NOT under Persistence/ 
 │     │     └── PostgreSQL/
 │     │           └── 20260307_AddBiReports.cs
 │     │
 │     ├── AI/
 │     │     └── OpenAIService.cs
 │     │
 │     ├── Services/
 │     └── DependencyInjection.cs
 │
 ├── WebAPI/
 │     ├── Endpoints/
 │     ├── Middleware/
 │     ├── Infrastructure/
 │     └── Program.cs
 │
 └── Shared/

------------------------------------------------------------
BACKEND RULES
------------------------------------------------------------

1. Domain layer:
   - Pure C#
   - No EF
   - No MediatR
   - No HTTP
   - No infrastructure reference

2. Application layer:
   - Only depends on Domain
   - Contains CQRS logic
   - Contains Interfaces
   - No DbContext usage

3. Infrastructure layer:
   - Implements interfaces
   - Contains EF Core
   - Contains OpenAI implementation
   - No business rules

4. WebAPI:
   - Thin endpoints only
   - No business logic
   - Only MediatR calls

------------------------------------------------------------
HOW TO ADD A NEW FEATURE — ACTUAL PATTERNS
------------------------------------------------------------

Example: Add "BiReports" module

1. Add Domain entity (NO sub-folder):
   Domain/Entities/BiReport.cs

   Rules:
   - inherit BaseAuditableEntity, ITenantEntity
   - public new Guid Id { get; set; }  (override int Id)
   - public Guid TenantId { get; set; }  (ITenantEntity)
   - all properties: public { get; set; }  (NO private set)
   - NO factory methods, NO domain methods
   - using MovithForms.Domain.Common; (for BaseAuditableEntity)

2. Add repository interface (flat, no sub-folders):
   Application/Common/Interfaces/IBiReportRepository.cs

3. Add Command (Command + Handler in ONE file):
   Application/Commands/BiReports/CreateBiReport.cs

   Inside ONE file:
   - [Authorize] on the Command class
   - Object initializer in Handler (NOT BiReport.Create())
   - Return Guid (entity Id)

4. Add Query (Query + Handler in ONE file):
   Application/Queries/BiReports/GetBiReports.cs

   Inside ONE file:
   - [Authorize] on the Query class
   - Handler uses repository interface

5. Add DTO (flat, no sub-folders):
   Application/DTOs/BiReportDto.cs

6. Implement repository in Infrastructure:
   Infrastructure/Repositories/BiReportRepository.cs

   namespace MovithForms.Infrastructure.Repositories;

   DO NOT put under Infrastructure/Persistence/Repositories/

7. Register in DI:
   Infrastructure/DependencyInjection.cs

8. Map endpoint:
   Web/Endpoints/BiReports.cs

FORBIDDEN PATTERNS:
- Application/BiReports/ (feature folder directly under Application)
- Application/Commands/BiReports/CreateBiReportCommand.cs (Command in separate file)
- Application/Commands/BiReports/CreateBiReportCommandHandler.cs (Handler in separate file)
- Application/BiReports/Queries/Dtos/BiReportDto.cs (nested DTOs)
- Domain/Entities/BiReports/BiReport.cs (entity in sub-folder)
- BiReport.Create() factory methods in domain entities
- Infrastructure/Persistence/Repositories/BiReportRepository.cs ← WRONG, must be Infrastructure/Repositories/
- Infrastructure/Persistence/Migrations/ ← WRONG, must be Infrastructure/Migrations/<Context>/
  Correct EF migration command:
  dotnet ef migrations add <Name> -p src/Infrastructure -s src/Web --context PostgreSQLDbContext --output-dir Migrations/PostgreSQL

------------------------------------------------------------
FRONTEND STRUCTURE (Next.js App Router)
------------------------------------------------------------

src/
 ├── app/
 │     ├── (auth)/
 │     ├── dashboard/
 │     ├── entry/
 │     ├── (forms)/
 │     ├── (operations)/
 │     └── (firm-admin)/
 │
 │
 ├── components/
 │     ├── ui/
 │     ├── layout/
 │     └── shared/
 │
 ├── hooks/
 │     ├── useTenant.ts
 │     ├── useSubscription.ts
 │     └── useFeatureFlag.ts
 │
 ├── services/
 │     ├── themeService.ts
 │
 ├── types/
 └── lib/

------------------------------------------------------------
FRONTEND RULES
------------------------------------------------------------

- No API call inside components
- Always use service layer
- Always use React Query
- Feature flags must be checked
- No business logic in UI
- No direct environment-based plan checks

------------------------------------------------------------
FORBIDDEN PATTERNS
------------------------------------------------------------

❌ Controller calling DbContext
❌ Handler calling HttpClient directly
❌ Entity referencing Infrastructure
❌ Feature logic inside Program.cs
❌ Plan logic inside frontend hardcoded
❌ Mixing Command & Query responsibilities
❌ Creating god-services
❌ Skipping TenantId filters

------------------------------------------------------------
ENFORCEMENT RULE
------------------------------------------------------------

If a request attempts to:

- Bypass layers
- Skip CQRS
- Skip multi-tenancy
- Add business logic in wrong layer

AI must refuse and provide a compliant solution.

------------------------------------------------------------

Architecture integrity is mandatory.

END OF DOCUMENT
