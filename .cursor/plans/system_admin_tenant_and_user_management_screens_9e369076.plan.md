---
name: System Admin Tenant and User Management Screens
overview: "Create two new admin screens for system administrators: Tenant management and User management screens using shadcn DataTable. Both screens will be placed in a new (system) directory and follow the same design pattern as the existing Tenant Users page."
todos:
  - id: install-dependencies
    content: Install @tanstack/react-table package and add shadcn table and select components
    status: completed
  - id: create-types
    content: Add TenantDTO interface to types.ts and extend user types if needed for tenant info
    status: completed
  - id: add-endpoints
    content: Add tenants endpoint definitions to endpoints.ts
    status: completed
  - id: create-route-handlers
    content: Create API route handlers for tenants (GET, POST, PUT, DELETE)
    status: completed
  - id: create-system-layout
    content: Create (system)/layout.tsx with Administrator role check
    status: completed
  - id: create-datatable-components
    content: Create reusable DataTable components (column-header, pagination, view-options)
    status: completed
  - id: create-tenant-schemas
    content: Create tenant validation schemas using zod
    status: completed
  - id: create-tenant-form
    content: Create TenantForm component with forwardRef support for FormDialog
    status: completed
  - id: create-tenant-page
    content: Create tenants/page.tsx with DataTable, CRUD operations, and FormDialog integration
    status: completed
  - id: create-user-page
    content: Create users/page.tsx with DataTable, reuse existing user management components
    status: completed
  - id: test-screens
    content: Test both screens with proper authorization and error handling
    status: completed
---

# System Admin Tenant and U

ser Management Screens

## Overview

Create two new management screens for system administrators (Administrator role):

1. **Tenant Management Screen** - Full CRUD operations for tenants
2. **User Management Screen** - View and manage all users across all tenants using existing components

Both screens will use shadcn DataTable for advanced table features (sorting, filtering, pagination, column visibility) and follow the design pattern of the existing Tenant Users page (`/tenant/users`).

## Project Structure

- New directory: `Frontend/src/app/(system)/` - Administrator-only routes
- `layout.tsx` - Layout with Administrator role check
- `tenants/page.tsx` - Tenant management screen
- `users/page.tsx` - User management screen

## Implementation Steps

### 1. Install Dependencies and Add Shadcn Components

**Package.json updates:**

- Add `@tanstack/react-table` package

**Shadcn components to add:**

- `npx shadcn@latest add table` - Base table component
- `npx shadcn@latest add select` - For pagination page size selector

### 2. Create Type Definitions

**`Frontend/src/lib/api/types.ts`**

- Add `TenantDTO` interface:
  ```typescript
        export interface TenantDTO {
          id: string;
          companyName: string;
          taxNumber: string;
          sector: string;
          email: string;
          isActive?: boolean;
          createdAt?: string;
          modifiedAt?: string;
        }
  ```




- Extend `UserDTO` or create extended interface if needed for system admin view (include tenant info)

### 3. Add API Endpoints Configuration

**`Frontend/src/lib/api/endpoints.ts`**

- Add `tenants` section:
  ```typescript
        tenants: {
          getAll: '/tenants',
          getById: (id: string) => `/tenants/${id}`,
          create: '/tenants',
          update: (id: string) => `/tenants/${id}`,
          delete: (id: string) => `/tenants/${id}`,
        },
  ```




### 4. Create Route Handlers (Backend Integration Ready)

**`Frontend/src/app/api/tenants/route.ts`** (new)

- GET handler - List all tenants
- POST handler - Create tenant

**`Frontend/src/app/api/tenants/[id]/route.ts`** (new)

- PUT handler - Update tenant
- DELETE handler - Delete tenant
- GET handler - Get tenant by ID

These route handlers will call backend endpoints when they are implemented.

### 5. Create (system) Layout

**`Frontend/src/app/(system)/layout.tsx`** (new)

- Similar to `(firm-admin)/layout.tsx`
- Use `useAuthGuard({ requiredSystemRoles: ["Administrator"] })`

### 6. Create DataTable Reusable Components

**`Frontend/src/components/data-table-column-header.tsx`** (new)

- Reusable column header component with sorting support (from shadcn docs)

**`Frontend/src/components/data-table-pagination.tsx`** (new)

- Reusable pagination component (from shadcn docs)

**`Frontend/src/components/data-table-view-options.tsx`** (new)

- Reusable column visibility toggle component (from shadcn docs)

### 7. Create Tenant Management Screen

**`Frontend/src/app/(system)/tenants/page.tsx`** (new)Features:

- DataTable with columns: Company Name, Tax Number, Sector, Email, Is Active, Actions
- "New Tenant" button
- Row actions dropdown menu: Edit, Delete
- FormDialog for create/update
- Uses TenantForm component

**`Frontend/src/components/TenantForm.tsx`** (new)

- Form component for tenant create/update
- Fields: Company Name, Tax Number, Sector, Email, Is Active (checkbox)
- Similar structure to UserUpdate component
- Uses forwardRef and useImperativeHandle for FormDialog integration
- Validation using zod schemas

**`Frontend/src/lib/schemas/tenantSchemas.ts`** (new)

- Zod schemas for tenant validation (CreateTenantSchema, UpdateTenantSchema)

### 8. Create User Management Screen

**`Frontend/src/app/(system)/users/page.tsx`** (new)Features:

- DataTable with columns: User Name, Email, Tenant (company name), System Roles, Actions
- Uses existing `/users` endpoint (GET /api/users - returns all users for Administrator)
- Row actions dropdown menu: Edit, Manage Roles, Change Password, Manage Notifications, Delete
- Reuses existing components:
- `UserUpdate` - for create/edit
- `AssignRoleForm` - for role management (will need tenant role assignment support)
- `ChangePassword` - for password changes
- `ManageNotificationPreferencesForm` - for notification preferences

Note: The existing UserUpdate, AssignRoleForm components may need minor adjustments to work with system admin context (no tenant filtering).

### 9. Design Consistency

Both screens should follow the same design pattern as `Frontend/src/app/(firm-admin)/tenant/users/page.tsx`:

- Same layout structure (breadcrumb, header with action button, white card container)
- Same FormDialog usage pattern
- Same dropdown menu structure for row actions
- Same toast notifications
- Same loading states

## Technical Considerations

1. **Backend Endpoints**: Tenant CRUD endpoints are not yet implemented. The frontend will be prepared with route handlers that will work once backend endpoints are added.
2. **User Management**: Uses existing `/api/users` endpoint which already returns all users for Administrator role. May need to extend UserDTO to include tenant information.
3. **Role Assignment**: The AssignRoleForm component may need to be extended to support both system roles and tenant roles, or we may need separate handling for system admin context.
4. **DataTable Features**: Implement sorting, filtering (by email/name), pagination, and column visibility toggles as per shadcn documentation.
5. **Authorization**: All screens and API routes should check for Administrator role.

## Files to Create

- `Frontend/src/app/(system)/layout.tsx`
- `Frontend/src/app/(system)/tenants/page.tsx`
- `Frontend/src/app/(system)/users/page.tsx`
- `Frontend/src/components/TenantForm.tsx`
- `Frontend/src/components/data-table-column-header.tsx`
- `Frontend/src/components/data-table-pagination.tsx`
- `Frontend/src/components/data-table-view-options.tsx`
- `Frontend/src/app/api/tenants/route.ts`
- `Frontend/src/app/api/tenants/[id]/route.ts`
- `Frontend/src/lib/schemas/tenantSchemas.ts`

## Files to Modify

- `Frontend/package.json` - Add @tanstack/react-table
- `Frontend/src/lib/api/types.ts` - Add TenantDTO
- `Frontend/src/lib/api/endpoints.ts` - Add tenants endpoints