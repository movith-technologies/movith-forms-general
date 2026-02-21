# PERFORMANCE SLO & SLA DEFINITION

## 1. OBJECTIVE

Define measurable service targets.

---

## 2. SLO DEFINITIONS

Availability SLO:
- 99.9% uptime

Latency SLO:
- p95 < 300ms (non-AI)
- p95 < 2s (AI endpoints)

Error Rate SLO:
- < 1% monthly

- Workflow processing SLA
- Submission completion SLA
- Dashboard query aggregation < 500ms target
- P95 < 1.5s

---

## 3. MEASUREMENT WINDOW

- Rolling 30-day window
- Measured continuously

---

## 4. ERROR BUDGET

Error budget = 0.1% downtime per month

If exceeded:

- Feature freeze
- Focus on reliability
- Root cause analysis required

---

## 5. SLA VS SLO

SLO:
- Internal target

SLA:
- Customer contractual guarantee

SLA must always be slightly lower than SLO.
