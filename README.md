# Movith Forms - No-Code Form Management & Workflow Platform

PoC implementation of a No-Code Form Management & Workflow Platform for manufacturing quality departments.

## Architecture

### Backend
- **.NET 9** with Clean Architecture (Onion Architecture)
- **Layers**: Domain, Application, Infrastructure, API
- **Patterns**: CQRS with MediatR, Repository Pattern
- **Database**: SQL Server / PostgreSQL (configurable)
- **Authentication**: ASP.NET Core Identity with JWT

### Frontend
- **Next.js 14+** (App Router)
- **TypeScript**
- **Tailwind CSS**
- **React Hook Form** for form handling

## Project Structure

```
Backend/
├── src/
│   ├── MovithForms.Domain/          # Domain entities and enums
│   ├── MovithForms.Application/     # Commands, Queries, DTOs, Interfaces
│   ├── MovithForms.Infrastructure/   # EF Core, Repositories, Services, Identity
│   └── MovithForms.Api/              # Controllers, Middleware, Configuration
└── MovithForms.sln

Frontend/
├── app/
│   ├── (admin)/                      # Admin UI pages
│   │   ├── login/
│   │   ├── form-definitions/
│   │   ├── workflow-definitions/
│   │   └── reporting/
│   └── (mobile)/                     # Mobile UI pages
│       ├── tracking/
│       ├── forms/
│       └── approvals/
├── contexts/                         # React contexts
├── lib/
│   └── api/                          # API client and types
└── package.json
```

## Setup Instructions

### Backend Setup

1. **Prerequisites**
   - .NET 9 SDK
   - SQL Server (LocalDB) or PostgreSQL
   - Visual Studio 2022 or VS Code

2. **Database Configuration**
   - Update `appsettings.json` with your connection string
   - Set `Database:Provider` to `"SqlServer"` or `"PostgreSQL"`

3. **Run Migrations**
   ```bash
   cd Backend/src/MovithForms.Api
   dotnet ef migrations add InitialCreate --project ../MovithForms.Infrastructure
   dotnet ef database update --project ../MovithForms.Infrastructure
   ```

4. **Run the API**
   ```bash
   cd Backend/src/MovithForms.Api
   dotnet run
   ```
   - API will be available at `https://localhost:7211/api` (or configured port)
   - Swagger UI: `https://localhost:7211/api/swagger`

### Frontend Setup

1. **Install Dependencies**
   ```bash
   cd Frontend
   npm install
   ```

2. **Configure API URL**
   - Create `.env.local`:
     ```
     NEXT_PUBLIC_BACKEND_API_URL=http://localhost:5000/api
     ```
   - Or update `lib/api/client.ts` with your API URL

3. **Run Development Server**
   ```bash
   npm run dev
   ```
   - Frontend will be available at `http://localhost:3000`

## Default Credentials

- **Username**: `admin@movith.com`
- **Password**: `Admin@123`
- **Role**: Admin

## Features Implemented

### Backend
- ✅ Authentication & Authorization (JWT + Identity)
- ✅ Form Definition CRUD
- ✅ Workflow Definition CRUD
- ✅ Form Instance Management
- ✅ Workflow Engine (Approve/Reject/Return)
- ✅ File Upload Service
- ✅ Reporting by Tracking

### Frontend
- ✅ Admin Login
- ✅ Form Definitions List
- ✅ Mobile Tracking Input
- ✅ Mobile Form Selection
- ✅ Dynamic Form Filling (from SchemaJson)
- ✅ File Upload
- ✅ Pending Approvals
- ✅ Reporting with Tracking Search

## API Endpoints

### Authentication
- `POST /api/auth/login` - Login and get JWT token

### Form Definitions
- `GET /api/form-definitions` - List all form definitions
- `GET /api/form-definitions/{id}` - Get form definition by ID
- `POST /api/form-definitions` - Create new form definition
- `PUT /api/form-definitions/{id}` - Update form definition
- `DELETE /api/form-definitions/{id}` - Delete form definition

### Workflow Definitions
- `GET /api/workflow-definitions` - List all workflow definitions
- `GET /api/workflow-definitions/{id}` - Get workflow definition by ID
- `POST /api/workflow-definitions` - Create new workflow definition
- `PUT /api/workflow-definitions/{id}` - Update workflow definition

### Forms
- `POST /api/forms/start` - Start a new form instance
- `GET /api/forms/{id}` - Get form instance by ID
- `POST /api/forms/{id}/submit` - Submit form data
- `POST /api/forms/{id}/actions` - Perform workflow action (Approve/Reject/Return)
- `GET /api/forms/by-tracking-no/{trackingNo}` - Get forms by tracking number
- `GET /api/forms/pending-approvals?role={role}` - Get pending approvals for a role

### Files
- `POST /api/files/upload` - Upload file/photo
- `GET /api/files/{filePath}` - Download file

## Form Schema Format

Form definitions use a JSON schema format:

```json
{
  "fields": [
    {
      "id": "field1",
      "type": "text",
      "label": "İş Emri No",
      "required": true
    },
    {
      "id": "field2",
      "type": "number",
      "label": "Miktar",
      "required": true
    },
    {
      "id": "field3",
      "type": "select",
      "label": "Durum",
      "options": ["Aktif", "Pasif"]
    }
  ]
}
```

Supported field types: `text`, `number`, `date`, `checkbox`, `select`, `upload`

## Notes

- This is a PoC implementation
- Multi-tenant, offline mode, and ERP/MES integration are out of scope
- UI is in Turkish as specified
- Code and API are in English

## License

Proprietary - Movith Technologies

