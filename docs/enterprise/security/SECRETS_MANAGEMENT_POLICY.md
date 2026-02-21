# SECRETS MANAGEMENT POLICY

## 1. PRINCIPLE

Secrets must never:

- Exist in source control
- Appear in logs
- Be hardcoded

---

## 2. STORAGE

Use centralized secret manager:

- Cloud Secret Manager
- Key Vault equivalent

Secrets include:

- DB connection strings
- JWT signing keys
- AI API keys
- Redis credentials
- SMTP credentials

---

## 3. ROTATION

- Secrets must support rotation
- AI keys must be rotatable
- Compromise requires immediate rotation

---

## 4. ACCESS CONTROL

Only backend service:

- May access secrets
- Never frontend

---

## 5. LOG REDACTION

Logs must redact:

- Tokens
- API keys
- Passwords
- Authorization headers
