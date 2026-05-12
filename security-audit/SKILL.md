---
name: security-audit
description: >
  OWASP Top 10 security audit. Scans code for vulnerabilities,
  hardcoded secrets, insecure patterns. Loads stack-specific reference.
  Use before deploy, after auth changes, or on demand.
---

# security-audit — OWASP Security Scanner

## Step 0: Context
Read `.ai/PROJECT_MAP.md` → understand data flows.
Read `.ai/SKILL_CONFIG.md` → load matching reference:
- Python → read `references/python-security.md`
- TS → read `references/ts-security.md`

## Procedure

### 1. Scope
Which modules? Auth? API? Data processing? Full scan?

### 2. OWASP Checklist

| # | Category | Check |
|---|----------|-------|
| A01 | Access Control | auth middleware on protected routes, resource-level checks, no IDOR, CORS not `*` |
| A02 | Crypto | passwords hashed (bcrypt/argon2), secrets in `.env`, HTTPS, JWT strong algo (RS256/ES256) |
| A03 | Injection | parameterized SQL, no user input concat, XSS escaping, no exec/eval with user data |
| A04 | Insecure Design | rate limiting on auth, file upload limits, request timeouts |
| A05 | Misconfiguration | security headers, debug OFF in prod, no stack traces to user |
| A06 | Vulnerable Deps | `pip audit` / `pnpm audit`, outdated deps, no deprecated libs |
| A07 | Auth Failures | session invalidation, refresh token rotation, password complexity |
| A08 | Data Integrity | no untrusted deserialization, server-side validation |
| A09 | Logging | auth failures logged, PII NOT logged, log injection prevented |

### 3. Secret scan
Grep for patterns:
```
AKIA[0-9A-Z]{16}
sk-[a-zA-Z0-9]{48}
ghp_[a-zA-Z0-9]{36}
password\s*[:=]\s*['"]
eyJ (JWT tokens)
```

Check: `.gitignore` has `.env`. Check git history: `git log --all -p -- '*.env'`

### 4. Stack-specific checks
Load reference → apply language-specific security rules.

### 5. Report
```
## 🔒 Отчёт безопасности
**Scope:** <modules> | **Дата:** <date> | **Оценка:** 🟢/🟡/🔴

### 🔴 Critical
- file:line — уязвимость → fix

### 🟡 High
- file:line — проблема → рекомендация

### 🟢 Medium
- file:line — замечание

### ✅ Проверено и ок
```

→ After: fix Critical items → re-audit
