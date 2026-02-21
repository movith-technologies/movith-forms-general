# ENGINEERING MASTER STANDARD

## 1. PURPOSE

Define engineering discipline for production-grade SaaS development.

Applies to:

- Backend
- Frontend
- AI integrations
- Infrastructure code

---

## 2. CLEAN ARCHITECTURE PRINCIPLE

System must follow:

- Clear separation of concerns
- No business logic in controllers
- No infrastructure leakage into domain layer
- Dependency inversion respected

---

## 3. CODE QUALITY REQUIREMENTS

Mandatory:

- Meaningful naming
- No magic strings
- No hardcoded tenant logic
- No cross-tenant assumptions
- No silent failures

---

## 4. AI INTEGRATION RULES

AI calls must:

- Go through centralized AI service
- Respect quota system
- Be logged
- Have timeout protection
- Have fallback handling

No direct AI calls from UI.

---

## 5. MULTI-TENANCY DISCIPLINE

Every query must include:

- TenantId filter

Never trust frontend tenant context.

Backend enforces isolation.

---

## 6. FEATURE IMPLEMENTATION CHECKLIST

Before merging:

- Security reviewed
- Plan gating implemented
- Logging added
- Monitoring impact evaluated
- AI cost impact considered (if applicable)

---

## 7. FORBIDDEN PRACTICES

❌ Direct DB access from UI  
❌ AI calls without quota check  
❌ Feature flags hardcoded  
❌ Cross-module coupling  
❌ Business logic in middleware  

---

## 8. PERFORMANCE AWARENESS

New code must:

- Avoid N+1 queries
- Avoid synchronous blocking
- Avoid excessive AI token usage
- Be horizontally scalable

---

## 9. TESTING REQUIREMENT

Minimum:

- Unit tests for domain logic
- Integration test for critical flows
- Tenant isolation validation tests
