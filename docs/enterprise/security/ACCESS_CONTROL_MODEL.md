# ACCESS CONTROL MODEL

## 1. AUTHENTICATION

Supported:
- JWT-based authentication
- OAuth2 compatible
- Token expiration enforced

Refresh tokens must:
- Be revocable
- Be stored securely

---

## 2. AUTHORIZATION MODEL

Hybrid:

- Role-Based Access Control (RBAC)
- Policy-Based Authorization

---

## 3. ROLE STRUCTURE

Example:

- TenantOwner
- Admin
- Manager
- User
- ReadOnly

Roles must be tenant-scoped.

---

## 4. POLICY ENFORCEMENT

All endpoints must:

- Validate authentication
- Validate role
- Validate TenantId match

Never rely on frontend role control.

---

## 5. COMPONENT-LEVEL AUTHORIZATION

Frontend:
- Feature gating via plan
Backend:
- Final enforcement

Backend is always source of truth.

---

## 6. FORBIDDEN PRACTICES

❌ Client-side authorization only  
❌ Role inference from UI  
❌ Hardcoded admin bypass  
❌ Cross-tenant service calls
