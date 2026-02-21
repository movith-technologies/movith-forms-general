# AI Agent Architecture Blueprint

Movith Forms is NOT an AI-native orchestration product.

AI usage is optional and supportive.

Primary architecture:

Web → Application → Domain → Infrastructure

If AI components are used:

- They must not own core business logic.
- They must not bypass tenant isolation.
- They must not access DB directly.
- They must operate through Application layer interfaces.

AI components may:
- Generate insights
- Detect anomalies
- Suggest workflow optimizations

But core workflow engine must remain deterministic.