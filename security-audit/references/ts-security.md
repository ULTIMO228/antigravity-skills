# TypeScript Security Reference

## Express / Node.js
- Helmet.js for security headers
- CORS: whitelist specific origins
- Input validation: zod / joi
- Rate limiting: express-rate-limit
- No eval() or Function() with user input

## Authentication
- bcrypt for password hashing (cost factor ≥ 12)
- JWT: RS256, short expiry (15min access, 7d refresh)
- Refresh token rotation on use
- HttpOnly + Secure + SameSite cookies

## Dependencies
```bash
pnpm audit          # Check vulnerabilities
pnpm outdated       # Check for updates
```

## Common Vulnerabilities
- XSS: sanitize user HTML (DOMPurify)
- CSRF: SameSite cookies + CSRF tokens
- Path traversal: validate file paths
- Prototype pollution: freeze objects, use Map
