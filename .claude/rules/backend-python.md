# Backend Python Rules

Apply these rules when editing Python backend files.

## Architecture
- Follow existing service boundaries strictly.
- Use async/await everywhere — this is a real-time system.
- Use dependency injection via FastAPI `Depends()` over global state.
- Keep functions focused, testable, and under 30 lines.

## Code style
- Use type hints on all function signatures and return types.
- Prefer explicit DTOs/Pydantic schemas over loose dictionaries.
- Add docstrings for non-obvious service and repository functions.
- Use clear, descriptive names for repository methods (e.g., `find_tickets_by_customer_id`).

## Patterns
- Never place business logic in route handlers — delegate to services.
- Use repository pattern for all database access.
- Use adapter pattern for all third-party integrations.
- Raise domain-specific exceptions, not generic ones.
- Return structured API responses with consistent error formats.

## Async and performance
- Never use blocking I/O in async handlers.
- Use `asyncio.gather()` for concurrent independent operations.
- Offload heavy processing (embeddings, transcription) to Celery workers.
- Use Redis for caching hot paths (customer context, knowledge base lookups).

## Logging
- Use structured JSON logging with correlation IDs.
- Log service-level decisions, not raw data.
- Never log PII, tokens, or credentials.