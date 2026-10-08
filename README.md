# Secure File Vault 🔒☁️

A zero-trust, production-grade private cloud file vault application built with strong cryptographic security, server-side envelope encryption, MFA, strict access control, and OWASP ASVS 5.0 compliance.

---

## 🌟 Key Features

- **🔐 Envelope Encryption**: Every file is encrypted using a unique random 256-bit Data Encryption Key (DEK) with **XChaCha20-Poly1305** before storage in MinIO.
- **🛡️ Cryptographic Shredding**: Deleting a file permanently destroys its DEK from the database, rendering ciphertext physically irrecoverable.
- **🔑 Argon2id Password Hashing**: State-of-the-art password security (64MB memory, 3 iterations, 4 parallelism threads).
- **📲 TOTP Multi-Factor Authentication**: RFC 6238 TOTP with QR code setup and single-use Argon2id hashed recovery codes.
- **📁 Full File & Folder Management**: Drag-and-drop upload, recursive folder navigation, search, soft delete (recycle bin), restore, and permanent deletion.
- **🔗 Time-Limited & Password-Protected Shares**: Create revocable public share links with optional password protection and maximum download limits.
- **📊 Security Audit Logging**: Detailed audit records tracking login, logout, MFA, file upload/download, deletion, and sharing events.
- **🛡️ OWASP ASVS 5.0 Baseline**: Protected against IDOR/BOLA, CSRF, XSS, Path Traversal, and SQL Injection.

---

## 🏗️ Architecture & Technology Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Frontend** | React 19 + TypeScript + Vite | Responsive dark-mode SPA with smooth micro-animations |
| **Backend** | Node.js 24 LTS + Fastify 5 | High-performance type-safe REST & WebSocket API |
| **Database** | PostgreSQL 18.x | Stores application metadata, session hashes, and encrypted DEKs |
| **Object Store** | MinIO | S3-compatible server storing encrypted `.enc` file objects |
| **Cryptography** | `libsodium-wrappers` & `argon2` | XChaCha20-Poly1305 envelope encryption & Argon2id |
| **Reverse Proxy**| Caddy | TLS termination and security headers (CSP, HSTS, CORS) |

---

## 🚀 Quick Start (Local Development)

### 1. Prerequisites
- [Node.js 24 LTS](https://nodejs.org/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows 11 / WSL2)

### 2. Infrastructure Setup (Docker Compose)
Start PostgreSQL, MinIO, and Caddy:
```bash
docker-compose up -d
```

### 3. Backend Setup
```bash
cd backend
npm install
npm run dev
```

### 4. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```

The application will be accessible at:
- **Frontend SPA**: `http://localhost:5173`
- **Backend API**: `http://localhost:4000/api/v1`
- **MinIO Console**: `http://localhost:9001` (Credentials in `.env.example`)

---

## 🧪 Testing

### Backend Unit & Cryptography Tests
```bash
cd backend
npm test
```

### End-to-End Tests
```bash
cd frontend
npm run test:e2e
```

---

## 📄 Documentation
- [Architecture & Implementation Plan](docs/architecture.md)
- [OWASP ASVS 5.0 Compliance Checklist](docs/security-checklist.md)
