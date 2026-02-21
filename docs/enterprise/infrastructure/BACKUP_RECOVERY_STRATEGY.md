# BACKUP & RECOVERY STRATEGY

## 1. OBJECTIVE

Prevent permanent data loss.

---

## 2. BACKUP TYPES

- Daily full backup
- Hourly incremental backup (optional)
- Point-in-time recovery enabled

---

## 3. RETENTION POLICY

Standard:
- 14 days

Pro:
- 30 days

Enterprise:
- Custom retention

---

## 4. STORAGE

Backups stored:

- Encrypted
- In separate storage account
- Different region (recommended)

---

## 5. RECOVERY OBJECTIVES

RPO:
- < 15 minutes (Enterprise)
- < 1 hour (Standard)

RTO:
- < 4 hours (Enterprise)
- < 12 hours (Standard)

---

## 6. TESTING

Backup restoration must be tested:

- Quarterly
- Logged and documented
