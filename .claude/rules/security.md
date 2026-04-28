# Security Rules

Apply these rules to all sensitive changes.

## Input and data handling
- Treat all external input as untrusted — validate and sanitize.
- Validate file uploads: check MIME types, file size, and content.
- Apply input length limits on all user-facing fields.
- Sanitize HTML/markdown content before rendering.

## Authentication and authorization
- Use JWT or session-based auth on all API endpoints.
- Implement role-based access control (RBAC): admin, supervisor, agent, customer.
- Apply least-privilege access patterns for all service accounts.
- Validate permissions on every request — never trust client-side role checks.

## PII and compliance
- Detect and mask PII in logs, analytics, and AI training data.
- Never log raw tokens, passwords, or credentials.
- Apply GDPR-compliant data handling: right to deletion, data export.
- Maintain audit logs for all privileged operations (user management, config changes).
- Support data retention policies with automated cleanup.

## Infrastructure security
- Avoid introducing unsafe shell execution.
- Keep integration credentials in a secrets manager — never in code or env files.
- Encrypt sensitive data at rest (AES-256) and in transit (TLS).
- Apply rate limiting on all public-facing endpoints.
- Use circuit breakers for external API calls.

## Incident response
- Flag any change that impacts auth, payments, or PII handling.
- Log security-relevant events with dedicated severity levels.
- Never expose stack traces, internal paths, or system details to end users.