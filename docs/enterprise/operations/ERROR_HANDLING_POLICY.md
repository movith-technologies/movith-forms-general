# ERROR HANDLING POLICY

## 1. OBJECTIVE

Provide predictable, safe, and debuggable error handling.

---

## 2. ERROR TYPES

- Validation errors
- Business logic errors
- Infrastructure errors
- AI provider errors
- Security errors

---

## 3. RESPONSE FORMAT

All API errors must return:

{
  "errorCode": "STRING_CODE",
  "message": "Human readable message",
  "correlationId": "GUID"
}

---

## 4. NO STACK TRACE EXPOSURE

Stack traces must:

- Never be returned to client
- Only logged internally

---

## 5. RETRY POLICY

Transient errors:

- Automatic retry (max 3)
- Exponential backoff

AI timeout:

- Graceful fallback
- Inform user

---

## 6. FAIL SAFE PRINCIPLE

If uncertain:

- Deny access
- Do not guess
- Do not auto-grant permissions
