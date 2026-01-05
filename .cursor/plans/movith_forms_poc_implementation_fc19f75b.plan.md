---
name: Movith Forms PoC Implementation
overview: Build a complete PoC for Movith Forms - a No-Code Form Management & Workflow Platform following Clean Architecture principles, with .NET 9 backend and Next.js 14 frontend.
todos:
  - id: backend-solution
    content: Create .NET 9 solution with 4 projects (Domain, Application, Infrastructure, Api) and configure project references
    status: completed
  - id: domain-entities
    content: Implement all domain entities (FormDefinition, WorkflowDefinition, WorkflowStep, FormInstance, FormSubmissionData, FormAttachment, WorkflowAction) with proper relationships
    status: completed
    dependencies:
      - backend-solution
  - id: domain-enums
    content: Create domain enums (FormStatus, WorkflowActionType, UserRole)
    status: completed
    dependencies:
      - backend-solution
  - id: application-commands
    content: Implement MediatR commands and handlers for FormDefinitions, Workflows, and Forms operations
    status: completed
    dependencies:
      - domain-entities
  - id: application-queries
    content: Implement MediatR queries and handlers for retrieving FormDefinitions, Workflows, and Forms data
    status: completed
    dependencies:
      - domain-entities
  - id: application-dtos
    content: Create DTOs and AutoMapper profiles for entity-to-DTO conversions
    status: completed
    dependencies:
      - domain-entities
  - id: infrastructure-dbcontext
    content: Create ApplicationDbContext with EF Core configurations for all entities, support both SQL Server and PostgreSQL
    status: completed
    dependencies:
      - domain-entities
  - id: infrastructure-repositories
    content: Implement repository classes for FormDefinition, WorkflowDefinition, and FormInstance
    status: completed
    dependencies:
      - infrastructure-dbcontext
  - id: infrastructure-services
    content: Implement FileStorageService for local file storage and JwtTokenService for token generation
    status: completed
    dependencies:
      - infrastructure-dbcontext
  - id: infrastructure-identity
    content: Configure ASP.NET Core Identity with JWT authentication, seed roles and admin user
    status: completed
    dependencies:
      - infrastructure-dbcontext
  - id: api-controllers
    content: Create API controllers (Auth, FormDefinitions, WorkflowDefinitions, Forms, Files) with proper authorization
    status: completed
    dependencies:
      - application-commands
      - application-queries
      - infrastructure-repositories
  - id: api-configuration
    content: Configure Program.cs with DI, middleware pipeline, Swagger, and database provider selection
    status: completed
    dependencies:
      - api-controllers
      - infrastructure-identity
  - id: frontend-setup
    content: Initialize Next.js 14+ project with TypeScript, Tailwind CSS, shadcn/ui, and set up project structure
    status: completed
  - id: frontend-api-client
    content: Create API client with authentication token handling and endpoint constants
    status: completed
    dependencies:
      - frontend-setup
  - id: admin-auth
    content: Implement admin login page and authentication context/provider
    status: completed
    dependencies:
      - frontend-api-client
  - id: admin-form-definitions
    content: Build Form Definitions CRUD pages with simple form builder UI (field palette, canvas, configuration panel)
    status: completed
    dependencies:
      - admin-auth
  - id: admin-workflow-definitions
    content: Build Workflow Definitions CRUD pages with step management UI
    status: completed
    dependencies:
      - admin-auth
  - id: admin-reporting
    content: Build reporting page with JobOrderNo search and FormInstance detail view with attachments and workflow history
    status: completed
    dependencies:
      - admin-auth
  - id: mobile-job-order
    content: Build mobile job order input page with Turkish labels
    status: completed
    dependencies:
      - frontend-api-client
  - id: mobile-form-selection
    content: Build mobile form selection page showing active forms for a job order
    status: completed
    dependencies:
      - mobile-job-order
  - id: mobile-form-filling
    content: Build dynamic form renderer that reads SchemaJson and renders fields, includes photo/file upload
    status: completed
    dependencies:
      - mobile-form-selection
  - id: mobile-approvals
    content: Build mobile approvals page with pending approvals list and action buttons (Approve/Reject/Return)
    status: completed
    dependencies:
      - mobile-job-order
  - id: workflow-engine
    content: Implement workflow state transitions (submit → first step, approve → next step, reject/return logic)
    status: completed
    dependencies:
      - api-controllers
  - id: file-upload-integration
    content: Complete file upload integration (backend accepts files, stores locally, frontend displays/downloads)
    status: completed
    dependencies:
      - infrastructure-services
      - mobile-form-filling
---

# Movith Forms PoC Implementation Plan

## Overview

Build a complete PoC demonstrating a No-Code Form Management & Workflow Platform for manufacturing quality departments. The system will have an Admin web UI for form/workflow definition and a mobile-friendly UI for operators on Zebra devices.

## Architecture Decisions

- **Backend**: .NET 9 with Clean Architecture (Domain, Application, Infrastructure, API layers)
- **Database**: Configurable SQL Server/PostgreSQL support via appsettings
- **Authentication**: ASP.NET Core Identity with JWT tokens
- **Frontend**: Next.js 14+ (App Router) with TypeScript, Tailwind CSS, shadcn/ui
- **Pattern**: CQRS with MediatR, Repository pattern

## Backend Implementation

### Phase 1: Solution Structure & Domain Layer

**1.1 Solution Setup**

- Create `MovithForms.sln` in `Backend/`
- Create 4 projects: Domain, Application, Infrastructure, Api
- Configure project references (Domain ← Application ← Infrastructure ← Api)
- Add NuGet packages: MediatR, FluentValidation, AutoMapper

**1.2 Domain Entities** (`Backend/src/MovithForms.Domain/Entities/`)

- `FormDefinition.cs` - Form template with SchemaJson
- `WorkflowDefinition.cs` - Workflow linked to FormDefinition
- `WorkflowStep.cs` - Ordered steps with roles
- `FormInstance.cs` - Concrete form submission linked to JobOrder
- `FormSubmissionData.cs` - JSON field data
- `FormAttachment.cs` - File/photos attached to forms
- `WorkflowAction.cs` - Approval/rejection history

**1.3 Domain Enums** (`Backend/src/MovithForms.Domain/Enums/`)

- `FormStatus.cs` (Draft, InReview, Approved, Rejected, Returned)
- `WorkflowActionType.cs` (Approve, Reject, Return)
- `UserRole.cs` (Admin, Operator, TeamLeader, QualityEngineer, Manager)

**1.4 Domain Interfaces**

- `IAuditableEntity.cs` - CreatedAt, CreatedBy, etc.
- `IDomainEvent.cs` - For future event sourcing

### Phase 2: Application Layer

**2.1 Commands** (`Backend/src/MovithForms.Application/Commands/`)

- `FormDefinitions/CreateFormDefinitionCommand.cs` + Handler
- `FormDefinitions/UpdateFormDefinitionCommand.cs` + Handler
- `FormDefinitions/DeleteFormDefinitionCommand.cs` + Handler
- `Workflows/CreateWorkflowDefinitionCommand.cs` + Handler
- `Workflows/UpdateWorkflowDefinitionCommand.cs` + Handler
- `Forms/StartFormInstanceCommand.cs` + Handler
- `Forms/SubmitFormDataCommand.cs` + Handler
- `Forms/PerformWorkflowActionCommand.cs` + Handler

**2.2 Queries** (`Backend/src/MovithForms.Application/Queries/`)

- `FormDefinitions/GetFormDefinitionQuery.cs` + Handler
- `FormDefinitions/GetAllFormDefinitionsQuery.cs` + Handler
- `Workflows/GetWorkflowDefinitionQuery.cs` + Handler
- `Forms/GetFormInstanceQuery.cs` + Handler
- `Forms/GetFormsByJobOrderQuery.cs` + Handler
- `Forms/GetPendingApprovalsQuery.cs` + Handler

**2.3 DTOs** (`Backend/src/MovithForms.Application/DTOs/`)

- `FormDefinitionDto.cs`
- `WorkflowDefinitionDto.cs`
- `FormInstanceDto.cs`
- `FormSubmissionDataDto.cs`
- `FormAttachmentDto.cs`
- `WorkflowActionDto.cs`
- `LoginRequestDto.cs` / `LoginResponseDto.cs`

**2.4 Mappings** (`Backend/src/MovithForms.Application/Mappings/`)

- `MappingProfile.cs` (AutoMapper) for Entity → DTO conversions

**2.5 Validation** (`Backend/src/MovithForms.Application/Validators/`)

- FluentValidation validators for all Commands

**2.6 Interfaces** (`Backend/src/MovithForms.Application/Interfaces/`)

- `IFormDefinitionRepository.cs`
- `IWorkflowDefinitionRepository.cs`
- `IFormInstanceRepository.cs`
- `IFileStorageService.cs`

### Phase 3: Infrastructure Layer

**3.1 Data Layer** (`Backend/src/MovithForms.Infrastructure/Data/`)

- `ApplicationDbContext.cs` - EF Core DbContext
- `Configurations/` - EF Core entity configurations (fluent API)
- `FormDefinitionConfiguration.cs`
- `WorkflowDefinitionConfiguration.cs`
- `FormInstanceConfiguration.cs`
- etc.
- Database provider abstraction for SQL Server/PostgreSQL

**3.2 Repositories** (`Backend/src/MovithForms.Infrastructure/Repositories/`)

- `FormDefinitionRepository.cs`
- `WorkflowDefinitionRepository.cs`
- `FormInstanceRepository.cs`
- All implement Application layer interfaces

**3.3 Services** (`Backend/src/MovithForms.Infrastructure/Services/`)

- `FileStorageService.cs` - Local file storage (wwwroot/uploads/)
- `JwtTokenService.cs` - JWT token generation

**3.4 Identity** (`Backend/src/MovithForms.Infrastructure/Identity/`)

- Configure ASP.NET Core Identity
- Seed initial admin user and roles
- JWT authentication configuration

**3.5 Migrations**

- Initial migration with all entities
- Seed data migration (roles, admin user)

### Phase 4: API Layer

**4.1 Controllers** (`Backend/src/MovithForms.Api/Controllers/`)

- `AuthController.cs` - `/api/auth/login`
- `FormDefinitionsController.cs` - `/api/form-definitions` (CRUD)
- `WorkflowDefinitionsController.cs` - `/api/workflow-definitions` (CRUD)
- `FormsController.cs` - `/api/forms/*` (start, submit, actions, by-job-order)
- `FilesController.cs` - `/api/files/upload`

**4.2 Middleware** (`Backend/src/MovithForms.Api/Middleware/`)

- `ErrorHandlingMiddleware.cs` - Global exception handling
- `RequestLoggingMiddleware.cs` - Optional logging

**4.3 Configuration** (`Backend/src/MovithForms.Api/`)

- `Program.cs` - Dependency injection, middleware pipeline
- `appsettings.json` - Database connection strings (SQL Server & PostgreSQL)
- `appsettings.Development.json`
- Swagger/OpenAPI configuration

**4.4 Authorization Policies**

- Role-based authorization attributes on controllers
- Policies for each role (Admin, Operator, TeamLeader, etc.)

## Frontend Implementation

### Phase 5: Next.js Project Setup

**5.1 Project Initialization**

- Initialize Next.js 14+ with TypeScript in `Frontend/`
- Install dependencies: Tailwind CSS, shadcn/ui, React Hook Form, Zod
- Configure `tailwind.config.ts`
- Set up `components.json` for shadcn/ui

**5.2 Project Structure**

- `app/` - Next.js App Router pages
- `(admin)/` - Admin route group
- `(mobile)/` - Mobile route group
- `components/` - Reusable components
- `features/` - Feature-based modules
- `lib/` - Utilities, API client
- `types/` - TypeScript types

**5.3 API Client** (`Frontend/lib/api/`)

- `client.ts` - Axios/fetch wrapper with auth token handling
- `endpoints.ts` - API endpoint constants
- `types.ts` - Frontend DTOs matching backend

### Phase 6: Admin UI

**6.1 Authentication** (`Frontend/app/(admin)/login/`)

- Login page with Turkish labels
- JWT token storage (localStorage/sessionStorage)
- Auth context/provider for role-based access

**6.2 Form Definitions** (`Frontend/app/(admin)/form-definitions/`)

- List page: Table of all FormDefinitions
- Create/Edit page:
- Form metadata (name, code, version, active)
- Form Builder UI:
- Left: Field type palette (Text, Number, Date, Checkbox, Select, Upload)
- Center: Canvas showing fields in order (drag & drop for PoC: simple add/remove)
- Right: Field configuration panel (label, required, validation)
- Save serializes to JSON schema → `SchemaJson`

**6.3 Workflow Definitions** (`Frontend/app/(admin)/workflow-definitions/`)

- List page: All workflows
- Create/Edit page:
- Select FormDefinition
- Manage ordered list of steps
- Each step: StepName, Role, IsFinalStep, CanReturnToPrevious
- Add/Remove/Reorder steps

**6.4 Reporting** (`Frontend/app/(admin)/reporting/`)

- Search by JobOrderNo
- List of FormInstances for that job
- Detail view:
- Form field data (rendered from DataJson)
- Attachments (images/files with preview)
- Workflow history timeline

### Phase 7: Mobile UI

**7.1 Job Order Input** (`Frontend/app/(mobile)/job-order/`)

- Simple input field for JobOrderNo
- (Future: QR/Barcode scanner button placeholder)
- Submit → redirect to form selection

**7.2 Form Selection** (`Frontend/app/(mobile)/forms/`)

- List active FormDefinitions for the job
- Large touch-friendly cards
- Select → navigate to form filling

**7.3 Dynamic Form Filling** (`Frontend/app/(mobile)/forms/[id]/fill`)

- Dynamic form renderer based on `SchemaJson`:
- Render fields by type (text, number, date, checkbox, select)
- React Hook Form for form state
- Validation based on schema
- Photo capture/upload button
- File upload button
- Submit button (large, finger-friendly)

**7.4 Approvals** (`Frontend/app/(mobile)/approvals/`)

- "My Pending Approvals" list
- Each item shows: FormInstance, JobOrderNo, CurrentStep
- Action buttons: Approve, Reject, Return
- Comment field for actions
- Workflow history view

## Integration & Polish

### Phase 8: Integration Points

**8.1 File Upload**

- Backend: Accept multipart/form-data
- Store in `wwwroot/uploads/{formInstanceId}/`
- Return file URL/path
- Frontend: Display images, download files

**8.2 Workflow Engine**

- On form submission → move to first workflow step
- On approval → move to next step (or complete if final)
- On rejection → mark as Rejected
- On return → move back to previous step (if allowed)

**8.3 Turkish Localization**

- All UI text in Turkish
- Error messages in Turkish
- Keep code/API in English

## Technical Notes

- **Form Schema Format**: JSON schema with `fields` array, each field has: `id`, `type`, `label`, `required`, `validation`, `options` (for select)
- **Data Storage**: FormSubmissionData.DataJson stores `{ "fieldId": "value" }` mapping
- **File Storage**: Local filesystem for PoC, structured as `uploads/{formInstanceId}/{filename}`
- **Authentication**: JWT tokens with role claims, stored in HTTP-only cookies or localStorage
- **Responsive Design**: Mobile UI uses Tailwind breakpoints, large touch targets (min 44x44px)

## Out of Scope (Future)

- Multi-tenant SaaS
- Offline mode/sync
- ERP/MES integration (JobOrderNo is just a string for now)
- Advanced reporting/analytics
- Multi-language support (beyond Turkish)
- Advanced form builder (drag & drop, conditional logic)