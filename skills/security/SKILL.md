---
name: security
description: Use when writing, reviewing, or modifying any code that handles user input, authentication, secrets, or data access. Covers input validation, parameterized queries, secret handling, and least privilege principles.
---

# Security Rules

> **Security is mandatory. Treat all external input as untrusted.**

---

## Input & Output

### Always

- Validate all inputs before processing
- Sanitize all outputs before rendering or returning
- Use parameterized queries — never string concatenation for SQL
- Handle authentication explicitly, never implicitly

### Never

- Trust client input without validation
- Bypass validation for convenience
- Expose internal errors to clients
- Log sensitive data (PII, tokens, passwords, secrets)

---

## Secrets & Credentials

### Always

- Use environment variables or secret managers for credentials
- Follow least privilege principle for all access
- Rotate credentials on a schedule

### Never

- Hardcode secrets in source code
- Hardcode secrets in configuration files
- Commit secrets to version control
- Log secrets or credentials
- Pass secrets in URLs or query parameters

---

## Authentication & Authorization

- Handle authentication explicitly on every protected endpoint
- Verify authorization before every sensitive operation
- Use established authentication patterns — do not invent new ones
- Fail securely — deny access when uncertain

---

## Data Protection

- Encrypt sensitive data at rest
- Use HTTPS for all external communication
- Validate data types and ranges, not just presence
- Implement rate limiting on public endpoints
- Use CSRF protection for state-changing operations

---

## Security Checklist

Before outputting any code:

- [ ] Are all inputs validated?
- [ ] Are all outputs sanitized?
- [ ] Are database queries parameterized?
- [ ] Are there any hardcoded secrets?
- [ ] Is authentication handled explicitly?
- [ ] Does this follow least privilege?
- [ ] Would this fail securely if something goes wrong?
- [ ] Are there any sensitive data in logs?

If any item fails, **fix it before outputting**.
