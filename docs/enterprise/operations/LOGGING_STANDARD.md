# LOGGING STANDARD

## 1. OBJECTIVE

Enable forensic debugging and audit capability.

---

## 2. LOG LEVELS

- Trace
- Debug
- Info
- Warning
- Error
- Critical

Production default:
- Info and above

---

## 3. STRUCTURED LOGGING

Logs must be:

- JSON structured
- Machine readable
- Include metadata

Required fields:

- Timestamp
- Level
- TenantId
- UserId
- CorrelationId
- Endpoint
- DurationMs

---

## 4. SECURITY REDACTION

Never log:

- Passwords
- Tokens
- API keys
- Authorization headers

---

## 5. RETENTION POLICY

Standard:
- 14 days

Pro:
- 30 days

Enterprise:
- 90+ days

---

## 6. IMMUTABILITY

Audit logs must:

- Be append-only
- Not editable
