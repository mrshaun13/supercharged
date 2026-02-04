# Security Guidelines - [PROJECT_NAME]

> **Source**: Adapted from [Project CodeGuard](https://github.com/project-codeguard/rules) - AI model-agnostic security framework

This document defines security practices that AI assistants and developers must follow. These are guidelines, not rigid rules—but deviations should be conscious and documented.

---

## Core Principles

1. **Secure by Default**: Build security in from the start, not as an afterthought
2. **Least Privilege**: Request only the permissions you need
3. **Defense in Depth**: Don't rely on a single security control
4. **Fail Securely**: Errors should not expose sensitive information

---

## Secrets Management

### ❌ Never Do
- Hardcode secrets, API keys, passwords, or tokens in source code
- Commit `.env` files with real credentials
- Log sensitive information
- Include secrets in error messages

### ✅ Always Do
- Use environment variables for secrets
- Add secret files to `.gitignore`
- Use secret management services in production (Vault, AWS Secrets Manager, etc.)
- Rotate secrets regularly

### Example `.gitignore` entries
```gitignore
# Secrets - NEVER commit these
.env
.env.local
.env.*.local
*.pem
*.key
secrets/
credentials.json
```

### Recognition Patterns - Learn to Spot These Formats

Common Secret Formats You Must NEVER Hardcode:

- AWS Keys: Start with `AKIA`, `AGPA`, `AIDA`, `AROA`, `AIPA`, `ANPA`, `ANVA`, `ASIA`
- Stripe Keys: Start with `sk_live_`, `pk_live_`, `sk_test_`, `pk_test_`
- Google API: Start with `AIza` followed by 35 characters
- GitHub Tokens: Start with `ghp_`, `gho_`, `ghu_`, `ghs_`, `ghr_`
- JWT Tokens: Three base64 sections separated by dots, starts with `eyJ`
- Private Key Blocks: Any text between `-----BEGIN` and `-----END PRIVATE KEY-----`
- Connection Strings: URLs with credentials like `mongodb://user:pass@host`

**Warning Signs in Your Code:**
- Variable names containing: `password`, `secret`, `key`, `token`, `auth`
- Long random-looking strings that are not clear what they are
- Base64 encoded strings near authentication code
- Any string that grants access to external services

### Example Environment Variable Pattern
```
# .env.example (commit this - no real values)
DATABASE_URL=postgresql://user:password@localhost/db
API_KEY=your-api-key-here
SECRET_KEY=generate-a-secure-random-key

# .env (do NOT commit - real values)
DATABASE_URL=postgresql://prod_user:real_password@prod-host/prod_db
API_KEY=sk-live-xxxxxxxxxxxxx
SECRET_KEY=a1b2c3d4e5f6...
```

---

## Input Validation & Injection Defense

### Core Strategy
- Validate early at trust boundaries with positive (allow‑list) validation and canonicalization.
- Treat all untrusted input as data, never as code. Use safe APIs that separate code from data.
- Parameterize queries/commands; escape only as last resort and context‑specific.

### Validation Playbook
- **Syntactic validation**: enforce format, type, ranges, and lengths for each field.
- **Semantic validation**: enforce business rules (e.g., start ≤ end date, enum allow‑lists).
- **Normalization**: canonicalize encodings before validation; validate complete strings (regex anchors ^$); beware ReDoS.
- **Free‑form text**: define character class allow‑lists; normalize Unicode; set length bounds.
- **Files**: validate by content type (magic), size caps, and safe extensions; server‑generate filenames; scan; store outside web root.

### ❌ Never Do
- Trust user input without validation
- Use string concatenation for SQL queries
- Directly embed user input in shell commands
- Render user input as HTML without escaping
- Use shell invocation for untrusted input
- Concatenate user input into LDAP queries without proper escaping

### ✅ Always Do
- Validate input type, length, format, and range
- Use parameterized queries for databases
- Sanitize input before use in commands
- Escape output based on context (HTML, URL, SQL, etc.)
- Apply context‑appropriate escaping for LDAP (DN and filter escaping)
- Use structured execution that separates command and arguments

### SQL Injection Prevention
```java
// GOOD - parameterized query
String custname = request.getParameter("customerName");
String query = "SELECT account_balance FROM user_data WHERE user_name = ? ";  
PreparedStatement pstmt = connection.prepareStatement( query );
pstmt.setString( 1, custname);
ResultSet results = pstmt.executeQuery();
```

### LDAP Injection Prevention
- Always apply context‑appropriate escaping:
  - DN escaping for `\ # + < > , ; " =` and leading/trailing spaces
  - Filter escaping for `* ( ) \ NUL`
- Validate inputs with allow‑lists before constructing queries

### OS Command Injection Defense
```java
// GOOD - structured execution
ProcessBuilder pb = new ProcessBuilder("TrustedCmd", "Arg1", "Arg2");
Map<String,String> env = pb.environment();
pb.directory(new File("TrustedDir"));
Process p = pb.start();
```

### Prototype Pollution (JavaScript)
- Use `new Set()` or `new Map()` instead of object literals
- Create objects with `Object.create(null)` or `{ __proto__: null }`
- Avoid unsafe deep merge utilities; block `__proto__`, `constructor`, `prototype`

### Implementation Checklist
- [ ] Central validators: types, ranges, lengths, enums; canonicalization before checks
- [ ] 100% parameterization coverage for SQL; dynamic identifiers via allow‑lists only
- [ ] LDAP DN/filter escaping in use; inputs validated prior to query
- [ ] No shell invocation for untrusted input; structured exec + allow‑list + regex validation
- [ ] JS object graph hardened: safe constructors, blocked prototype paths, safe merge utilities
- [ ] File uploads validated by content, size, and extension; stored outside web root and scanned

---

## Authentication & Authorization

### Authentication Guidelines
- [ ] Use established authentication libraries (don't roll your own)
- [ ] Enforce strong password policies (minimum 8 characters, passphrases accepted)
- [ ] Check new passwords against breach databases and reject common passwords
- [ ] Implement account lockout and rate limiting after failed attempts
- [ ] Use secure session management with HttpOnly, SameSite cookies
- [ ] Always return generic error messages to prevent account enumeration
- [ ] Support password managers (allow paste, no JavaScript blocks)

### Password Storage
- Hash passwords with slow, memory‑hard algorithms (Argon2id preferred, bcrypt/scrypt acceptable)
- Use unique per‑user salts and constant‑time comparison
- Never encrypt passwords - always hash them
- Consider using a pepper (stored separately from database) for additional security

### Multi‑Factor Authentication (MFA)
- Require MFA for sensitive accounts and operations
- Prefer phishing‑resistant factors (WebAuthn/passkeys, hardware tokens)
- Accept TOTP apps as backup; avoid SMS/voice codes when possible
- Implement secure recovery with backup codes
- Require MFA for: login, password changes, privilege elevation, new devices

### Authorization Guidelines
- [ ] Check permissions on every request
- [ ] Use role-based (RBAC) or attribute-based (ABAC) access control
- [ ] Validate user owns resources they're accessing (prevent IDOR)
- [ ] Log access to sensitive resources
- [ ] Apply least privilege principle

### Federation & APIs
- Use standard protocols (OAuth 2.0, OIDC, SAML) - don't build your own
- Validate redirect URIs, state parameters, and tokens properly
- Use short-lived tokens with proper rotation
- Prefer opaque tokens over JWT when possible for easier revocation
- Implement rate limiting on all authentication endpoints

---

## Cryptography

### ❌ Never Do
- Implement your own cryptographic algorithms
- Use deprecated algorithms (MD5, SHA1 for security, DES, RC4)
- Use ECB mode for encryption
- Hardcode encryption keys

### ✅ Always Do
- Use well-vetted cryptographic libraries
- Use strong algorithms (AES-256, SHA-256+, RSA-2048+)
- Use authenticated encryption (GCM mode)
- Properly manage key lifecycle

### Recommended Algorithms
| Purpose | Recommended | Avoid |
|---------|-------------|-------|
| Hashing (passwords) | bcrypt, Argon2, scrypt | MD5, SHA1, plain SHA256 |
| Hashing (data integrity) | SHA-256, SHA-3 | MD5, SHA1 |
| Symmetric encryption | AES-256-GCM | DES, 3DES, RC4, AES-ECB |
| Asymmetric encryption | RSA-2048+, ECDSA | RSA-1024 |

---

## Data Protection

### Sensitive Data Handling
- Encrypt sensitive data at rest
- Use TLS for data in transit
- Minimize data collection (collect only what's needed)
- Implement data retention policies
- Mask sensitive data in logs and UI

### PII (Personally Identifiable Information)
- [ ] Identify what PII the application handles
- [ ] Document where PII is stored
- [ ] Encrypt PII at rest
- [ ] Implement access controls for PII
- [ ] Support data deletion requests

---

## Dependency Security

### ❌ Never Do
- Use dependencies with known critical vulnerabilities
- Pin to exact versions without security update strategy
- Import dependencies from untrusted sources

### ✅ Always Do
- Regularly audit dependencies for vulnerabilities
- Keep dependencies updated (especially security patches)
- Use lockfiles for reproducible builds
- Verify package integrity (checksums, signatures)

### Recommended Tools
| Ecosystem | Tool |
|-----------|------|
| JavaScript/Node | `npm audit`, Snyk, Dependabot |
| Python | `pip-audit`, Safety, Dependabot |
| Go | `govulncheck`, Dependabot |
| General | OWASP Dependency-Check |

---

## Error Handling & Logging

### Error Messages
- Don't expose stack traces to users in production
- Don't reveal system information in errors
- Log full details internally, show generic messages externally

### Secure Logging
```
# BAD - logs sensitive data
logger.info(f"User login: {username}, password: {password}")

# GOOD - logs safely
logger.info(f"User login attempt: {username}")
```

### What to Log
- [ ] Authentication attempts (success and failure)
- [ ] Authorization failures
- [ ] Input validation failures
- [ ] Application errors
- [ ] Security-relevant events

### What NOT to Log
- Passwords or secrets
- Full credit card numbers
- Personal health information
- Session tokens
- Encryption keys

---

## API Security

### REST API Guidelines
- [ ] Use HTTPS only
- [ ] Implement rate limiting
- [ ] Validate Content-Type headers
- [ ] Return appropriate status codes
- [ ] Don't expose internal IDs if possible (use UUIDs)

### Authentication Options
| Method | Use Case |
|--------|----------|
| API Keys | Server-to-server, simple auth |
| OAuth 2.0 | User authorization, third-party access |
| JWT | Stateless authentication |

### CORS Configuration
- Be specific about allowed origins (not `*` in production)
- Limit allowed methods and headers
- Consider credentials requirements

---

## Security Checklist for New Features

### Before Implementation
- [ ] Identify sensitive data involved
- [ ] Document authentication/authorization requirements
- [ ] Consider attack vectors (STRIDE, OWASP Top 10)

### During Implementation
- [ ] Validate all inputs
- [ ] Use parameterized queries
- [ ] Apply least privilege
- [ ] Handle errors securely

### Before Release
- [ ] Run security-focused code review
- [ ] Scan dependencies for vulnerabilities
- [ ] Test authentication/authorization
- [ ] Verify secrets are not in code

---

## Resources

- **Project CodeGuard**: https://github.com/project-codeguard/rules
- **OWASP Top 10**: https://owasp.org/Top10/
- **OWASP Cheat Sheets**: https://cheatsheetseries.owasp.org/
- **CWE (Common Weakness Enumeration)**: https://cwe.mitre.org/

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | [Date] | Initial security guidelines |
