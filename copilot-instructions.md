# Omni-FI Core - Copilot Coding Guidelines

## Context
Omni-FI is a B2B financial data aggregation platform ("The Plaid of Mauritius"). We use a hybrid architecture of API reverse-engineering and Playwright HTML scraping.

## Global Coding Standards
1. **Naming Conventions:** 
   - ALWAYS use `snake_case` for PostgreSQL database columns and Django model fields. 
   - ALWAYS use `PascalCase` for JSON API responses to strictly align with UK Open Banking (OBIE) standards.
2. **Type Hinting:** Use strict Python type hints for all function signatures and return types.

## Security & Infrastructure Rules (CRITICAL)
1. **Credential Handling:** NEVER write code that logs plaintext bank passwords, API keys, or raw HTML DOMs that might contain user account numbers.
2. **Encryption:** All bank credentials must be handled using the established Envelope Encryption (KMS DEK/KEK) pattern in `apps.credential_vault`.
3. **Tenant Isolation:** All database queries must enforce the `Client-Connection-Token` architecture. Queries must be scoped to the `ApiClient` via middleware. Do NOT use the legacy `User` model for data ownership.

## Scraping Engine Rules
1. **Playwright Resilience:** When writing Playwright code, always assume selectors will fail. Implement safe extraction using `page.locator().inner_text()` wrapped in `try/except` blocks.
2. **Stealth:** All browser automation must utilize the proxy manager and stealth plugins.
3. **SBM Specifics:** When writing code for SBM, implement the "Smart Login Circuit Breaker" logic to gracefully handle `User Already Logged In` errors without crashing the worker.
