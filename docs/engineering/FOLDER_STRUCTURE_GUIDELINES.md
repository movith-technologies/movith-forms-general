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
 │     ├── Entities/
 │     ├── ValueObjects/
 │     ├── Enums/
 │     ├── Events/
 │     └── Common/
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
 │     │     ├── Repositories/
 │     │     └── ApplicationDbContext.cs
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
 │     ├── Filters/
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
HOW TO ADD A NEW FEATURE
------------------------------------------------------------

Example: Add "Auction" module

1. Create folder:
   Application/Features/Auctions/

2. Inside:
   - Commands/
   - Queries/
   - DTOs/
   - Validators/

3. Add Domain entity:
   Domain/Entities/Auction.cs

4. Add repository interface:
   Application/Common/Interfaces/IAuctionRepository.cs

5. Implement in Infrastructure

Do NOT:
- Put business logic in Controllers
- Create random Services folder in Application
- Access DbContext directly in Handler

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
