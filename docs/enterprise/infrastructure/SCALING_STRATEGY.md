# SCALING STRATEGY

## 1. PRINCIPLE

System must scale horizontally.

No vertical-only scaling dependency.

---

## 2. API SCALING

Stateless API:
- Horizontal auto-scaling
- CPU and request-based scaling triggers

---

## 3. DATABASE SCALING

Approach:

Stage 1:
- Single DB with indexing optimization

Stage 2:
- Read replicas

Stage 3:
- Tenant-based sharding

Enterprise:
- Dedicated DB per tenant (optional)

---

## 4. CACHE STRATEGY

Redis used for:

- Session caching
- AI response caching
- Rate limiting
- Feature flags

---

## 5. AI SCALING

AI calls must:

- Be async where possible
- Use queue for heavy workloads
- Enforce concurrency limits per tenant

---

## 6. BOTTLENECK AVOIDANCE

Monitor:

- DB locks
- CPU spikes
- Token spikes
- Memory pressure
