# FRONTEND CODING STANDARD
# Project: MOVITH FORMS SaaS
# Stack: Next.js (App Router) + TypeScript + Tailwind + React Query

This document defines mandatory frontend coding rules.
All AI agents must strictly comply.

Architecture stability > UI creativity.

------------------------------------------------------------
1. GENERAL PRINCIPLES
------------------------------------------------------------

- TypeScript strict mode enabled.
- No "any" type unless justified.
- Components must be small and composable.
- No business logic inside UI components.
- UI must reflect backend rules — not override them.
- Multi-tenant awareness is mandatory.
- Server Components by default
- Client components only for charts / interaction
- No direct fetch inside component body
- All API calls via typed service layer
- Dashboard charts reusable

------------------------------------------------------------
2. PROJECT STRUCTURE
------------------------------------------------------------

app/
  (auth)/
  (dashboard)/
  api/
  layout.tsx
  page.tsx

components/
  ui/
  shared/
  forms/
  dashboard/

lib/
  api/
  hooks/
  utils/
  auth/

No random folders.
No feature logic inside app root.

------------------------------------------------------------
3. SERVER vs CLIENT RULES
------------------------------------------------------------

Default = Server Component

Use "use client" ONLY when:
- Using hooks (useState, useEffect)
- Handling form state
- Using browser APIs

Never:
- Fetch sensitive data in client if it can be fetched server-side
- Store business-critical logic in client

Server is source of truth.

------------------------------------------------------------
4. DATA FETCHING RULES
------------------------------------------------------------

Use:

- Server Actions (when suitable)
- React Query for client-side dynamic data

Never:
- Call fetch directly inside random components
- Hardcode API URLs
- Bypass central API client

All API calls must go through:

lib/api/client.ts

------------------------------------------------------------
5. API CLIENT RULE
------------------------------------------------------------

Create centralized wrapper:

- Handles base URL
- Injects auth token
- Handles 401 redirect
- Handles tenant header

No component should manually attach tokens.

------------------------------------------------------------
6. STATE MANAGEMENT
------------------------------------------------------------

Local state:
useState (component-level)

Server state:
React Query

Global UI state:
Context (minimal usage)

Never:
- Use Redux unless necessary
- Store server data in local state manually

------------------------------------------------------------
7. FEATURE-BASED ARCHITECTURE
------------------------------------------------------------

Each feature folder must contain:

- components/
- hooks/
- services/
- types.ts

Example:

features/forms/
  components/
  hooks/
  forms.service.ts
  types.ts

Do not mix feature logic across folders.

------------------------------------------------------------
8. ROLE & PLAN AWARENESS
------------------------------------------------------------

Frontend must:

- Hide unauthorized features
- Respect plan-based gating
- Respect feature flags

But:

Backend always validates again.

Frontend gating = UX improvement only.

------------------------------------------------------------
9. AI MODULE RULES
------------------------------------------------------------

AI features must:

- Be isolated under features/ai
- Call backend AI endpoints
- Never call OpenAI directly
- Show token usage if applicable
- Gracefully handle AI failure

AI is enhancement — not system dependency.

------------------------------------------------------------
10. FORM RULES
------------------------------------------------------------

Use:

- React Hook Form
- Zod for schema validation

Validation must mirror backend validation.

No manual form state chaos.

------------------------------------------------------------
11. COMPONENT RULES
------------------------------------------------------------

Max 150 lines per component.
Split into smaller pieces if needed.

Presentational components:
- No business logic

Container components:
- Handle data fetching

Naming:

NotificationBell.tsx
PendingApprovalsBadge.tsx
SubscriptionPlanCard.tsx

------------------------------------------------------------
12. STYLING RULES
------------------------------------------------------------

- Tailwind only
- No inline styles
- No CSS modules unless necessary
- No random color usage

Use design tokens if defined.

Enterprise > fancy gradients.

------------------------------------------------------------
13. PERFORMANCE RULES
------------------------------------------------------------

- Use dynamic imports for heavy modules
- Avoid unnecessary client components
- Memoize expensive components
- Use proper keys in lists
- Avoid unnecessary re-renders

------------------------------------------------------------
14. ERROR HANDLING
------------------------------------------------------------

- Use centralized error UI component
- No silent failures
- Show user-friendly messages
- Log technical errors

------------------------------------------------------------
15. AUTHENTICATION RULES
------------------------------------------------------------

- Auth handled via secure cookies
- No token in localStorage
- No manual token parsing in components
- Logout must clear cache

------------------------------------------------------------
16. MULTI-TENANCY ENFORCEMENT
------------------------------------------------------------

Frontend must:

- Always send tenant context
- Never allow manual tenant switching via URL manipulation
- Not expose other tenant data

Tenant awareness must be invisible but enforced.

------------------------------------------------------------
17. CODE SMELLS (FORBIDDEN)
------------------------------------------------------------

❌ Business logic in UI
❌ 300+ line components
❌ Direct OpenAI calls
❌ Hardcoded plan logic
❌ Magic strings for roles
❌ API calls inside deeply nested components
❌ Ignoring loading states
❌ Ignoring error states

------------------------------------------------------------
18. TESTABILITY RULE
------------------------------------------------------------

- Hooks must be testable
- Services must be isolated
- UI must be deterministic

------------------------------------------------------------
19. INTERNATIONALIZATION (FUTURE SAFE)
------------------------------------------------------------

All user-facing strings must be:

- Extractable
- Not hardcoded deeply
- i18n-ready

System must support:

- Turkish (primary)
- English (secondary)
- Future expansion

------------------------------------------------------------
20. ENTERPRISE UI PRINCIPLE
------------------------------------------------------------

Design must prioritize:

- Clarity
- Readability
- Data density
- Performance

Not animation-heavy landing page aesthetics.

------------------------------------------------------------
FINAL RULE
------------------------------------------------------------

If a frontend solution:

- Breaks multi-tenancy
- Bypasses backend authority
- Hardcodes subscription logic
- Couples AI logic to UI

AI must reject it and propose compliant alternative.

Consistency > Flashiness
Scalability > Convenience
Security > Speed

END OF DOCUMENT
