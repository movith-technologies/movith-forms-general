# PROMPT ENGINEERING RULES

## 1. STRUCTURE

Prompt must include:

1. System context
2. Role definition
3. Constraints
4. Output format rules

---

## 2. INJECTION PROTECTION

Sanitize:

- HTML
- Scripts
- System override attempts

Reject malicious patterns.

---

## 3. OUTPUT CONTROL

Require:

- Structured JSON when needed
- Deterministic formatting
- No hidden instructions

---

## 4. PROMPT VERSIONING

All prompts must:

- Have version ID
- Be stored centrally
- Be testable

---

## 5. TOKEN OPTIMIZATION

Avoid:

- Redundant context
- Long conversation history
- Unnecessary memory injection
