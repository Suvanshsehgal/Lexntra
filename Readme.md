# Lexntra — AI-Powered Legal Document Generation Platform

> **Live**: [Frontend](https://lex-port.vercel.app) · [API](https://lex-port.onrender.com)
>
> **Stack**: React 19 · Express 5 · MongoDB · Groq AI · Puppeteer · Cloudinary

Lexntra is a full-stack platform that generates professional legal PDFs (Rent Agreements, NDAs, Freelance Contracts, Partnership Agreements, Service Agreements) from guided forms, augmented by an AI legal assistant and lawyer consultation booking.

---

## Highlights

- **Polymorphic schema design** — 5 legal document types share a single MongoDB collection via Mongoose discriminators, with per-type validation and a unified query interface
- **Server-side PDF pipeline** — Form data → EJS template → Puppeteer headless Chrome → A4-formatted PDF download with embedded digital signatures, typography, and print CSS
- **Domain-restricted AI chatbot** — Real-time legal Q&A via Groq Cloud's Llama 3.1-8B, with server-enforced system prompt limiting to legal-only queries
- **End-to-end auth security** — bcrypt pre-save hooks, JWT Bearer tokens (1h expiry), protected routes, Mongoose injection protection, EJS output escaping
- **Clean layered architecture** — Controllers, routes, middlewares, utilities, and models separated with async error handling, custom error classes, and standardized API responses
- **Production deployed** — Backend on Render (Puppeteer Chrome via postinstall), frontend on Vercel, CORS whitelist, environment-driven config

---

## Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 19, Vite 7, React Router DOM v7, Tailwind CSS 3, Axios (JWT interceptor), lucide-react |
| **Backend** | Node.js, Express 5, Mongoose (discriminators), JWT, bcryptjs, EJS, Puppeteer, Multer |
| **AI** | Groq Cloud API (Llama 3.1-8B instant), server-side domain enforcement |
| **Media** | Cloudinary (signature upload, PDF hosting) |
| **Email** | Nodemailer (Gmail SMTP) |
| **Infrastructure** | Render.com (backend), Vercel (frontend) |

---

## Architecture

```
React Form → FormData → Axios POST (JWT) → Multer → Auth Middleware
  → Controller → Cloudinary (signature) → Mongoose (discriminator save)
    → EJS render → Puppeteer (headless Chrome → A4 PDF)
      → Response stream → Temp file cleanup
```

### Database: Mongoose Discriminators

All 5 document types inherit from a `LegalDocument` base schema keyed by `DocumentType`:

- **Base**: `{ DocumentType, user, DocumentCreationDate }`
- **RentAgreement** — landlord, tenant, property, rent, lease, signatures
- **NdaAgreement** — parties, binding agreement, signatures
- **FreelanceAgreement** — client, freelancer, compensation, jurisdiction
- **PartnershipAgreement** — company, partner, product info, payment, legal
- **ServiceAgreement** — customer, services, compensation, contract term

Single collection (`legaldocuments`), queryable by type, with per-type Mongoose validation.

### API Endpoints

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/user/register` | — | Create account |
| POST | `/api/v1/user/login` | — | Login, returns JWT |
| POST | `/api/v1/documents` | Bearer JWT | Submit form → generate PDF |
| GET | `/api/v1/documents/history` | Bearer JWT | User's document history |
| POST | `/api/v1/consultation/book` | — | Book lawyer consultation (email) |
| POST | `/api/v1/chat` | — | AI legal chatbot query |

---

## Getting Started

```bash
# Backend
cd backend
npm install
cp .env.example .env   # fill in secrets
npm run dev            # localhost:5000

# Frontend
cd frontend
npm install
npm run dev            # localhost:5173
```

**Environment** — MongoDB URI, JWT secret, Cloudinary credentials, Groq API key, Gmail app password.

---

## Key Design Decisions

- **Why discriminators?** A single collection with polymorphic schemas avoids N collections with near-identical queries, enables unified history aggregation, and keeps the schema flexible for new document types
- **Why server-side PDF?** Puppeteer + EJS gives pixel-perfect A4 output with custom fonts, headers, and signature placement — browser `print()` cannot match this fidelity or automation
- **Why server-enforced AI guardrails?** The legal domain requires strict boundaries; the system prompt is enforced server-side, not client-side, preventing prompt injection
- **Why no cookies?** JWT Bearer tokens avoid CSRF entirely and align with the SPA + mobile API consumption pattern

---

## Deployment

- **Backend** — Render Web Service: `npm install` (postinstall downloads Chrome) → `npm start`
- **Frontend** — Vercel: root `frontend`, build `npm run build`, output `dist`


