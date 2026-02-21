# BREAKING CHANGE POLICY

## 1. OBJECTIVE

Prevent production disruption for active tenants.

---

## 2. DEFINITION

Breaking change includes:

- API contract modification
- Removed field
- Changed response format
- Permission rule alteration

---

## 3. REQUIREMENTS

Before breaking change:

- Major version bump
- Deprecation notice
- Migration documentation
- Grace period defined

---

## 4. ENTERPRISE NOTICE

Enterprise tenants must receive:

- Minimum 30-day notice
- Dedicated migration support if required

---

## 5. ROLLBACK

Every breaking deployment must:

- Have rollback plan
- Have tested fallback version
