---
name: security-audit
description: >
  Используй когда нужно проверить код на уязвимости: перед деплоем, после реализации
  аутентификации/авторизации, при работе с пользовательскими данными, или по запросу
  пользователя. Проверяет по OWASP Top 10 и выдаёт отчёт с приоритетами.
---

# security-audit

## Procedure

1. Scope: which modules? Read `.ai/PROJECT_MAP.md` → understand data flows.
2. **OWASP Checklist**:

| Category | Check |
|----------|-------|
| A01 Access Control | auth middleware on protected routes, resource-level checks, no IDOR, CORS not `*` |
| A02 Crypto | passwords hashed (bcrypt/argon2), secrets in `.env`, HTTPS, JWT strong algo (RS256/ES256), no PII in JWT |
| A03 Injection | parameterized SQL, no user input concat in queries, XSS escaping, no exec/eval with user data, NoSQL input validation |
| A04 Insecure Design | rate limiting on auth, file upload limits+type validation, request timeouts, login attempt limiting |
| A05 Misconfiguration | security headers (CSP, X-Frame-Options:DENY, X-Content-Type-Options:nosniff, HSTS), debug OFF in prod, no stack traces to user |
| A06 Vulnerable Deps | `pnpm audit`, outdated deps with security patches, no deprecated libs with CVEs, lock file committed |
| A07 Auth Failures | session invalidation on logout, refresh token rotation, password complexity, same error msg for wrong email/password |
| A08 Data Integrity | no untrusted deserialization, server-side validation, file upload content-type validation |
| A09 Logging | auth failures logged, PII NOT logged, log injection prevented |

3. **Secret scan**: search for `AKIA[0-9A-Z]{16}`, `sk-[a-zA-Z0-9]{48}`, `ghp_[a-zA-Z0-9]{36}`, `password\s*[:=]\s*['"]`, `eyJ...`. Check `.gitignore` has `.env`. Check git history: `git log --all -p -- '*.env'`
4. **Report**:
```
## 🔒 Отчёт безопасности
**Scope:** <> | **Дата:** <> | **Оценка:** 🟢/🟡/🔴
### 🔴 Critical
- file:line — уязвимость → fix
### 🟡 High / 🟢 Medium
- file:line — проблема → рекомендация
### ✅ Проверено и ок
```
