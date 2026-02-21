# AGENT EXECUTION GUARDRAILS

## 1. OBJECTIVE

Prevent uncontrolled or unsafe AI agent behavior.

---

## 2. EXECUTION LIMITS

Agent must have:

- Max recursion depth
- Max tool calls per session
- Max execution time

Hard stop required.

---

## 3. TOOL ACCESS CONTROL

Each tool must declare:

- Required role
- Tenant scope
- Risk level

Agents cannot dynamically invent tools.

---

## 4. SYSTEM PROMPT PRIORITY

Hierarchy:

1. System instructions (highest)
2. Developer instructions
3. User input (lowest)

User input must never override system rules.

---

## 5. FORBIDDEN ACTIONS

Agent cannot:

- Access secrets
- Execute system commands
- Access other tenant data
- Modify billing limits

---

## 6. HUMAN ESCALATION

High-risk operations require:

- Human confirmation
- Explicit approval

---

## 7. AUDIT LOGGING

Each agent session must log:

- TenantId
- UserId
- Tools used
- Token consumption
- Execution duration
