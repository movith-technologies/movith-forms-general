# API RATE LIMITING POLICY

## 1. OBJECTIVE

Protect:

- Infrastructure
- AI cost
- System stability

---

## 2. LIMIT TYPES

Per:

- IP
- User
- Tenant
- API key

---

## 3. RATE LIMIT STRATEGY

Examples:

Free Tier:
- 60 requests/min

Pro:
- 300 requests/min

Enterprise:
- Custom SLA-based

AI endpoints must have stricter limits.

---

## 4. RESPONSE

When exceeded:

- HTTP 429
- Clear error message
- Retry-after header

---

## 5. PROTECTION AGAINST BURSTS

Use:

- Sliding window
- Token bucket algorithm

---

## 6. MONITORING

Track:

- Per-tenant usage
- Spike detection
- Abuse patterns
