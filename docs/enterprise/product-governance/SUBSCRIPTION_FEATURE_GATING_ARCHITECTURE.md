# Subscription Feature Gating Architecture

Feature access must:

- Be evaluated in Application layer
- Be tenant-aware
- Support dynamic updates
- Be cacheable

Forbidden:

- UI-based gating
- Hardcoded boolean checks
- Plan logic in controllers