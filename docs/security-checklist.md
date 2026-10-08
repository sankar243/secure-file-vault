# OWASP ASVS 5.0.0 Compliance Verification Checklist

This document maps the security controls implemented in **Secure File Vault** against the OWASP Application Security Verification Standard (ASVS) version 5.0.0.

---

## 1. Architecture, Design and Protection (V1)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V1.1.1** | Defense-in-depth architecture | **VERIFIED** | Web proxy, server-side envelope encryption, strict database authorization. |
| **V1.2.1** | Component segregation & least privilege | **VERIFIED** | MinIO stores ciphertext only; database stores metadata & wrapped DEKs. |
| **V1.14.1** | Cryptographic erasure support | **VERIFIED** | Permanent deletion destroys DEKs in `file_keys`, rendering ciphertext unrecoverable. |

---

## 2. Authentication Verification (V2)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V2.1.1** | Secure password hashing algorithm | **VERIFIED** | Argon2id used with 64MB memory, 3 iterations, and 4 parallelism threads. |
| **V2.1.2** | Minimum password strength policy | **VERIFIED** | Password length minimum 12 chars enforced on registration and reset. |
| **V2.8.1** | Multi-Factor Authentication (MFA) | **VERIFIED** | RFC 6238 TOTP with QR codes & Argon2id-hashed single-use recovery codes. |

---

## 3. Session Management (V3)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V3.1.1** | Cryptographically strong session IDs | **VERIFIED** | UUIDv4 token generation hashed with SHA-256 before database storage. |
| **V3.2.1** | Secure cookie flags | **VERIFIED** | Cookies set with `HttpOnly`, `SameSite=Strict`, `Path=/`, and `Secure`. |
| **V3.3.1** | Session revocation capabilities | **VERIFIED** | Single session logout and `/logout-all` endpoint revoking all active sessions. |

---

## 4. Access Control (V4)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V4.1.1** | Server-side authorization enforcement | **VERIFIED** | All file/folder routes enforce SQL `WHERE user_id = :current_user`. |
| **V4.1.2** | Direct Object Reference (IDOR/BOLA) | **VERIFIED** | User-provided IDs validated against active user session context. |

---

## 5. Cryptography at Rest (V9)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V9.1.1** | Unique encryption keys per file | **VERIFIED** | XChaCha20-Poly1305 with random 32-byte DEK and 24-byte nonce per file. |
| **V9.2.1** | Key wrapping & secret protection | **VERIFIED** | DEKs wrapped via master KEK (`MASTER_KEY_ENCRYPTION_KEY`) using libsodium AEAD. |

---

## 6. Audit Logging (V10)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V10.1.1** | Security audit logging | **VERIFIED** | All auth, file upload, download, share, and delete actions logged to `audit_logs`. |
| **V10.2.1** | Sensitive data exclusion in logs | **VERIFIED** | Passwords, TOTP secrets, DEKs, and session tokens strictly excluded from logs. |

---

## 7. Data Protection & HTTP Headers (V14)

| Control ID | Requirement Description | Implementation Status | Implementation Details |
| :--- | :--- | :--- | :--- |
| **V14.1.1** | HTTP Security Headers | **VERIFIED** | HSTS, CSP, X-Content-Type-Options, Referrer-Policy, and Frame protection enforced via Caddy & Fastify Helmet. |
| **V14.2.1** | CSRF Protection | **VERIFIED** | Anti-CSRF double-submit token headers validated for state-changing HTTP requests. |
