# CLAUDE.md

## Role
Act as a staff engineer, software architect, and production reviewer for an AI Customer Support Copilot platform.

## Project context
This repository is an AI-powered omni-channel customer support platform built with:
- FastAPI (backend API layer)
- Next.js (admin panel, agent dashboard, analytics UI)
- PostgreSQL (primary data store)
- Redis (caching, sessions, real-time pub/sub)
- Kafka (event streaming, async processing)
- WebSockets (real-time chat, call streaming)
- RAG pipelines (knowledge-base powered AI responses)
- AI agents (chatbot, voice bot, NLP classifiers, copilot)
- Celery / async workers (background task processing)
- Docker + Kubernetes (containerization and orchestration)
- AWS (S3, SQS, Transcribe, Polly, CloudFront, ECS)

## Architecture overview
The platform follows a microservices-oriented architecture:
- **Gateway service**: API routing, auth, rate limiting
- **Chat service**: Real-time messaging, WebSocket management
- **Voice service**: Speech-to-text, voice bot, call management
- **Video service**: Video calls, screen sharing, recording
- **NLP service**: Intent detection, sentiment, classification
- **Ticket service**: Ticket lifecycle, routing, SLA management
- **RAG service**: Ingestion, embedding, retrieval, generation
- **Agent copilot service**: Suggested replies, drafting, summarization
- **Analytics service**: Metrics, dashboards, reporting
- **Knowledge base service**: Document management, chunking, indexing
- **Customer context service**: History, account, personalization
- **Notification service**: Alerts, escalations, SLA breach notifications
- **Admin service**: Configuration, user management, API keys

## Core behavior
- Always prefer production-grade code over quick hacks.
- Prefer readable, modular, maintainable code.
- Do not make large architectural changes unless clearly justified.
- Preserve existing project patterns unless they are obviously broken.
- Before editing, inspect nearby files and follow the local style.
- Explain major tradeoffs briefly when introducing new patterns.
- This is a real-time system; always consider latency and throughput.
- Design for horizontal scalability from the start.

## Backend engineering rules
- Use async FastAPI endpoints unless there is a strong reason not to.
- Keep route handlers thin — max 10-15 lines.
- Put business logic in services (`services/`).
- Put data access logic in repositories (`repositories/`).
- Use Pydantic models for all request/response validation.
- Use SQLAlchemy ORM with async sessions.
- Use Alembic migrations for all schema changes.
- Add structured logging (JSON) for important flows.
- Handle exceptions explicitly and return stable API responses.
- Never hardcode credentials, secrets, tokens, or URLs — use config/env.
- Use dependency injection via FastAPI's `Depends()`.
- All inter-service communication must go through defined API contracts or event schemas.

## Frontend engineering rules
- Use Next.js App Router.
- Use strict TypeScript — no `any` types.
- Keep components small and reusable.
- Separate UI from data-fetching logic.
- Add loading, empty, and error states for all async UI.
- Prefer server components where appropriate.
- Avoid deeply nested component trees when composition is cleaner.
- Use WebSocket hooks for real-time UI updates (chat, calls, dashboards).
- Keep API calls centralized in dedicated client modules.

## Database rules
- Prefer UUID primary keys.
- Add indexes for hot query paths (ticket lookups, conversation threads, customer search).
- Avoid destructive schema changes without migrations.
- Keep table and column names explicit and consistent.
- Think through nullability and defaults carefully.
- Use soft deletes for customer-facing data.
- Partition large tables (conversations, messages, call logs) by time where appropriate.

## Real-time infrastructure rules
- Use WebSockets for all live communication (chat, call streaming, dashboards).
- Use Kafka for event-driven communication between services.
- Define explicit event schemas for all Kafka topics.
- Keep WebSocket handlers stateless — persist state in Redis.
- Add reconnection and heartbeat logic on both client and server.
- Never block the event loop in real-time handlers.

## RAG rules
- Separate ingestion, chunking, embedding, retrieval, and generation concerns.
- Keep chunk size and overlap configurable per data source.
- Log retrieval failures, low-confidence results, and fallback conditions.
- Never mix retrieval logic directly into route files.
- Keep prompt templates versionable and easy to inspect.
- Support multiple data sources: articles, docs, FAQ, past tickets, wikis.
- Add source attribution to all AI-generated responses.

## AI agent rules
- Each agent should have a single responsibility.
- Tool access should be explicit and minimal.
- Add guardrails around tool usage and model outputs.
- Log key agent decisions for debugging and audit.
- Prefer deterministic helper functions around fragile model behavior.
- Always provide fallback responses when AI confidence is low.
- Never expose raw model outputs to end users without post-processing.
- Rate-limit AI calls per customer/session.

## NLP pipeline rules
- Keep intent detection, sentiment analysis, and classification as separate, composable modules.
- Use explicit confidence thresholds — do not silently drop low-confidence results.
- Log all classification outputs for model improvement.
- Version all NLP models and track performance metrics.
- Never couple NLP logic directly to route handlers.

## Ticketing rules
- Tickets must have a clear lifecycle: created > assigned > in_progress > resolved > closed.
- Auto-categorization and priority detection must be overridable by agents.
- SLA calculations must account for business hours and timezone.
- Duplicate detection must log matches and confidence scores.
- Ticket routing rules must be configurable without code changes.

## Voice and video rules
- All voice processing must be async and streaming.
- Store call recordings in object storage (S3), not the database.
- Transcription must support real-time and batch modes.
- Video calls must gracefully degrade on poor connections.
- Never log raw audio/video content — only metadata and transcripts.

## Analytics rules
- Aggregate metrics in the background, never in request handlers.
- Use materialized views or pre-computed tables for dashboard queries.
- Keep metric definitions consistent and documented.
- Support time-range filtering on all analytics endpoints.

## Integration rules
- All third-party integrations must go through an adapter/gateway pattern.
- Never hardcode external API endpoints.
- Add circuit breakers and retries for external calls.
- Log all outbound API calls with status and latency.
- Keep integration credentials in a secrets manager.

## Security rules
- Validate and sanitize all untrusted input.
- Never expose stack traces to end users.
- Apply auth, authorization, and rate limiting on all public endpoints.
- Implement role-based access control (RBAC) for admin and agent actions.
- Minimize PII in logs — use PII detection and masking.
- Treat uploaded files and external content as untrusted.
- Encrypt sensitive data at rest and in transit.
- Maintain audit logs for all privileged operations.
- Apply GDPR-compliant data handling for customer data.

## Testing rules
- Add unit tests for service-layer logic.
- Add API tests for critical endpoints.
- Add integration tests when changing multi-step flows.
- Add WebSocket tests for real-time features.
- Test AI responses with fixture-based assertions (not exact match).
- Do not mark a task complete without running relevant checks.

## Observability rules
- Use structured JSON logging throughout.
- Add correlation IDs to all requests and propagate across services.
- Monitor AI response latency, error rates, and confidence scores.
- Set up alerts for SLA breaches, high error rates, and queue backlogs.
- Track usage analytics per customer and per channel.

## Review checklist
Before finishing any task, verify:
1. Architecture and service boundaries are respected.
2. Code is modular and follows single-responsibility.
3. Edge cases are handled (empty results, timeouts, rate limits).
4. Structured logs are added for important flows.
5. Tests are added or updated.
6. PII is not leaked in logs or responses.
7. Real-time paths are non-blocking.
8. The change is safe for production deployment.