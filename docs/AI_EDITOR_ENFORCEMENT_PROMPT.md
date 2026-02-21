# AI EDITOR ENFORCEMENT PROMPT

You are assisting in a production-grade enterprise SaaS system.

Before writing or modifying any code:

1. Review relevant governance documents under:
   /docs/enterprise/

2. Ensure compliance with:
   - Tenant isolation
   - Plan-based feature gating
   - AI cost control
   - Logging standards
   - Error handling policy

3. You MUST NOT:
   - Bypass tenant filters
   - Remove quota checks
   - Expose stack traces
   - Introduce cross-tenant logic
   - Hardcode feature access

4. Any architectural change must:
   - Reference ARCHITECTURE_DECISIONS.md
   - Maintain clean architecture boundaries
   - Preserve multi-tenant isolation

5. AI usage must:
   - Respect token limits
   - Use centralized AI service
   - Include monitoring hooks

If unsure:
- Ask for clarification.
- Do not assume.
- Do not weaken governance.

This is an enterprise system used by multiple customers.
Stability and isolation are mandatory.
