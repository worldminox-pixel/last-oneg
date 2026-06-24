# Valora Financial Bank (VFB)

An ultra-modern, offshore mobile-banking platform and wealth clearance experience. Built with a full-stack architecture using a React + Vite frontend and an Express backend, backed by secure Firestore database persistence.

---

## 🚀 Key Features

- **Strategic Sovereignty & Security**: Interactive dashboards highlighting asset custody, high-yield vaults, secure multi-account wallets, and instant bank peer-to-peer transfers.
- **Certified Receipts with VFB Bank Seal**: Fully printable deposit and wire transition receipts containing the custom gold-and-navy SVG Valora logo.
- **Tawk.to Live Customer Chat Component**: Dynamically adjusted widget positions depending on the view. It sits in the bottom-right corner of public landing or login websites and adjusts to sit directly above the mobile bottom navigation menu inside core dashboards.
- **Resilient Firestore Dual-Layer Persistence**: Features automated 5-second connection limits, failure-resistant failovers, local state memory mirroring (`valora-state.json`), and high-speed write loops to prevent hangs or service degradation.
- **Secure Email Gateway Module**: Prepared configuration mapping to deliver 2FA validation codes, OTP authorizations, and secure clearance instructions via SMTP protocols.

---

## 🛠️ Environment Variables Configuration

Valora Financial Bank uses server-side execution variables to safely process database state transitions, execute secure operations, and manage SMTP configurations without leaking variables to the client-side browser.

Below is the list of active environmental keys, their descriptions, and the references mapped within the server layers:

### 1. Configuration File
Environment variables are officially declared in **`/.env.example`** at the project root. Create or update your **`.env`** file matching the keys below:

```env
# ==========================================
# CORE AI STUDIO PARAMETERS
# ==========================================
GEMINI_API_KEY="MY_GEMINI_API_KEY"
APP_URL="MY_APP_URL"

# ==========================================
# SECURE SMTP MAIL GATEWAY CONFIGURATION
# ==========================================
SMTP_HOST="mail.privategateway.ch"
SMTP_PORT="587"
SMTP_SECURE="false" # Set to "true" for port 465 (SSL), or "false" for 587/25 (TLS)
SMTP_USER="security@valora-trust.ch"
SMTP_PASS="VaultSecurityPass123"
SMTP_FROM='"Valora Trust Security" <security@valora-trust.ch>'
```

### 2. Detailed Variable Mapping

| Environment Variable | Primary File Mapped | Description & Function |
| :--- | :--- | :--- |
| **`GEMINI_API_KEY`** | `/server.ts` | Configures key authorizations when invoking Gemini LLM models for text-parsing or clearance analysis. Automatically mapped via AI Studio's Secrets Manager. |
| **`APP_URL`** | `/server.ts` | The hosted base Domain URL (e.g., Cloud Run public domain). Powering dynamic loops, self-referential hooks, and redirect routes. |
| **`SMTP_HOST`** | `/server.ts` | IP address or domain hostname of the private SMTP message dispatcher. Used for certified OTP 2FA or system alerts. |
| **`SMTP_PORT`** | `/server.ts` | Network port used to initiate connection channels with the security SMTP server (typically `587` or `465`). |
| **`SMTP_SECURE`** | `/server.ts` | Controls secure socket layer mapping. Evaluated as `"true"` for SSL channels or `"false"` for TLS handshake protocols. |
| **`SMTP_USER`** | `/server.ts` | Account username required to authenticate sender identity during certified transaction dispatches. |
| **`SMTP_PASS`** | `/server.ts` | Secure authorization password protecting credentialed SMTP delivery channels. |
| **`SMTP_FROM`** | `/server.ts` | Custom sender header string (e.g. `Valora Trust Security <security@valora-trust.ch>`) displayed on client inbox alerts. |

---

## 📦 Project Assembly & Execution

The system is compiled using standard Node.js module structures.

### Development Mode
Boot standard fast local servers executing fully-compiled TypeScript:
```bash
npm run dev
```

### Production Build Sequence
Compiles client-side bundles into static `dist/` folders and packages backend layers into a self-contained runtime:
```bash
# Production compile & bundle
npm run build

# Start server node runner
npm start
```

---

## 🏛️ Firestore Database Rules Configuration
For external services, setup rules using `/firestore.rules`:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /valora_bank_data/{document} {
      allow read, write: if true; // Custom sandbox state persistence parameters
    }
  }
}
```
