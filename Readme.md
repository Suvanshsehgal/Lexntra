# Lexntra — AI-Powered Legal Document Generation Platform

> **Automate. Consult. Generate.** — Transform complex legal paperwork into polished PDFs in seconds.

Lexntra is a full-stack application that lets users generate legally-formatted documents (Rent Agreements, NDAs, Freelance Contracts, Partnership Agreements, Service Agreements) through an intuitive form interface, backed by an AI legal assistant and lawyer consultation booking.

---

## ✨ Features

<dl>
<dt><strong>5 Legal Document Generators</strong></dt>
<dd>Rent Agreement, NDA, Freelance Contract, Partnership Agreement, Service Agreement — each with its own form fields and professionally formatted PDF output.</dd>

<dt><strong>AI Legal Chatbot</strong></dt>
<dd>Powered by Groq Cloud's LLaMA 3.1-8B model. Restricted to legal-only queries via system prompt enforcement on the server side.</dd>

<dt><strong>PDF Generation Pipeline</strong></dt>
<dd>Form data → EJS template → Puppeteer headless Chrome → A4 PDF download. Temp files are cleaned up after delivery.</dd>

<dt><strong>Digital Signature Upload</strong></dt>
<dd>Upload signature images via Cloudinary integration, embedded directly into generated documents.</dd>

<dt><strong>User Authentication</strong></dt>
<dd>JWT-based register/login with bcrypt password hashing. Protected routes for document submission and history.</dd>

<dt><strong>Document History</strong></dt>
<dd>Per-user history of all generated documents, sorted by creation date.</dd>

<dt><strong>Lawyer Consultation Booking</strong></dt>
<dd>Browse lawyer profiles by specialization, book consultations via form → email notification via Nodemailer (Gmail SMTP).</dd>

<dt><strong>Responsive UI</strong></dt>
<dd>React SPA with Tailwind CSS, dark green (#1e463c) brand theme, smooth scroll navigation, and mobile-friendly layout.</dd>
</dl>

---

## 🧱 Tech Stack

### Backend

| Technology | Purpose |
|---|---|
| **Node.js** | JavaScript runtime |
| **Express 5** | HTTP server and routing |
| **MongoDB + Mongoose** | Database with ODM (discriminator-based schema for polymorphic document types) |
| **JWT (jsonwebtoken)** | Stateless authentication tokens (1h expiry) |
| **bcryptjs** | Password hashing (salt rounds: 10) |
| **Puppeteer** | Headless Chrome for A4 PDF generation |
| **EJS** | Templating engine for PDF rendering |
| **Cloudinary** | Cloud image upload for signatures |
| **Nodemailer** | Email service via Gmail SMTP |
| **Multer** | Multipart file upload middleware |
| **Axios** | HTTP client for Groq Cloud API |
| **dotenv** | Environment variable management |

### Frontend

| Technology | Purpose |
|---|---|
| **React 19** | UI library |
| **Vite 7** | Build tool and dev server (HMR) |
| **React Router DOM v7** | Client-side routing |
| **Tailwind CSS 3** | Utility-first CSS framework |
| **Axios** | HTTP client with JWT interceptor |
| **lucide-react** | Icon library |
| **react-scroll** | Smooth scroll navigation |

### External Services

| Service | Integration |
|---|---|
| **Groq Cloud API** | LLaMA 3.1-8B for legal chatbot |
| **Cloudinary** | Signature image hosting |
| **Gmail SMTP** | Consultation email delivery |
| **Render.com** | Backend hosting |
| **Vercel** | Frontend hosting |

---

## 🏗 Project Structure

```
Lexntra/
├── backend/
│   ├── server.js                     # Entry point
│   ├── app.js                        # Express app setup
│   ├── db/
│   │   └── index.js                  # MongoDB connection
│   ├── models/
│   │   ├── user.models.js            # User schema
│   │   ├── form.models.js            # LegalDocument base (discriminator)
│   │   ├── rentAgreement.js          # Rent agreement discriminator
│   │   ├── ndaAgreement.js           # NDA discriminator
│   │   ├── freelanceAgree.model.js   # Freelance discriminator
│   │   ├── partnerAgree.model.js     # Partnership discriminator
│   │   └── serviceAgree.model.js     # Service agreement discriminator
│   ├── controllers/
│   │   ├── auth.controller.js        # Register/login logic
│   │   ├── form.controller.js        # Document submission & history
│   │   └── consultation.controller.js# Consultation booking
│   ├── routes/
│   │   ├── user.routes.js            # /api/v1/user/*
│   │   ├── form.routes.js            # /api/v1/documents/*
│   │   ├── consultation.routes.js    # /api/v1/consultation/*
│   │   └── chatbot.routes.js         # /api/v1/chat
│   ├── middlewares/
│   │   ├── auth.middleware.js        # JWT verification
│   │   ├── error.middleware.js       # Global error handler
│   │   └── multer.middleware.js      # File upload handler
│   ├── utils/
│   │   ├── asyncHandler.js           # Async error wrapper
│   │   ├── ApiError.js               # Custom error class
│   │   ├── ApiResponse.js            # Standard response format
│   │   ├── cloudinary.js             # Cloudinary upload helpers
│   │   ├── emailService.js           # Nodemailer transporter
│   │   └── puppeteerHelper.js        # PDF generation from EJS
│   └── templates/                    # EJS templates for PDFs
│       ├── rentAggrement.template.ejs
│       ├── ndaAggrement.template.ejs
│       ├── freelanceAgreement.template.ejs
│       ├── partnershipAgreement.template.ejs
│       └── serviceAgreement.template.ejs
├── frontend/
│   ├── index.html
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   └── src/
│       ├── main.jsx                  # React root with router
│       ├── App.jsx                   # Root component
│       ├── api.js                    # Axios instance + JWT interceptor
│       ├── pages/
│       │   ├── LandingPage.jsx
│       │   ├── Login.jsx
│       │   ├── Signup.jsx
│       │   ├── Home.jsx
│       │   ├── Profile.jsx
│       │   ├── Consult.jsx
│       │   ├── RentAggrementForm.jsx
│       │   ├── NdaAgree.jsx
│       │   ├── FreelanceAgree.jsx
│       │   ├── PartnershipAgreement.jsx
│       │   └── ServiceAgreeForm.jsx
│       └── compoents/
│           ├── Navbar.jsx
│           ├── HeroSection.jsx
│           ├── Cards.jsx
│           ├── Chat.jsx
│           ├── History.jsx
│           ├── LawyerCard.jsx
│           └── Form components...
└── project-analysis.md (documentation)
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 18
- **MongoDB** (local or Atlas)
- **A Cloudinary account** (free tier)
- **A Groq Cloud API key** (free tier)
- **A Gmail account** with app password for Nodemailer

### 1. Clone & Install Dependencies

```bash
git clone https://github.com/your-username/lexntra.git
cd lexntra

# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

### 2. Configure Environment Variables

Create `backend/.env`:

```env
PORT=5000
MONGODB_URI=mongodb+srv://<user>:<pass>@cluster0.xxxxx.mongodb.net
DB_NAME=lexntra

JWT_SECRET=your_jwt_secret_key_here

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

GROQ_API_KEY=gsk_your_groq_api_key

EMAIL_USER=your_email@gmail.com
EMAIL_PASSWORD=your_gmail_app_password
```

> ⚠️ Never commit `.env` to version control. Add it to `.gitignore`.

### 3. Run Locally

```bash
# Terminal 1 — Backend (port 5000)
cd backend
npm run dev

# Terminal 2 — Frontend (port 5173)
cd frontend
npm run dev
```

Visit **http://localhost:5173** in your browser.

> **CORS note**: The frontend dev server on port 5173 is whitelisted in `backend/app.js`. If you change ports, update the `allowedOrigins` array.

---

## 🔌 API Reference

### Authentication

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/user/register` | — | Create account (`{ Username, email, password }`) |
| POST | `/api/v1/user/login` | — | Login, returns JWT (`{ email, password }`) |

### Documents

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/documents` | Bearer JWT | Submit form data (multipart), returns PDF |
| GET | `/api/v1/documents/history` | Bearer JWT | List user's generated documents |

### Consultation & Chat

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/v1/consultation/book` | — | Book lawyer consultation (sends email) |
| POST | `/api/v1/chat` | — | Send message to AI legal chatbot |

### Response Format

**Success:**
```json
{ "success": true, "message": "...", "data": { ... } }
```

**Error:**
```json
{ "success": false, "message": "...", "code": 400 }
```

---

## 🗄 Database Schema

### Collection: `users`

| Field | Type | Constraints |
|---|---|---|
| `_id` | ObjectId | Primary key |
| `Username` | String | **Unique**, required, indexed |
| `email` | String | **Unique**, required, indexed |
| `password` | String | Required, bcrypt-hashed |
| `referencetoken` | [String] | — |

### Collection: `legaldocuments`

Single collection using **Mongoose discriminators**, keyed by `DocumentType`.

| Field | Type | Notes |
|---|---|---|
| `_id` | ObjectId | Primary key |
| `DocumentType` | String | Discriminator key — `RentAgreement`, `NdaAgreement`, etc. |
| `user` | ObjectId (ref: User) | Indexed for history queries |
| `DocumentCreationDate` | Date | — |
| `__t` | String | Mongoose internal discriminator type |

Each document type extends the base with its own fields (party details, property info, compensation, signatures, etc.)

---

## 📄 PDF Generation Pipeline

```
React Form → FormData → Axios POST (JWT) → Multer (file upload)
  → Auth Middleware (JWT verify) → Controller
    → Cloudinary (signature upload)
    → Mongoose (save to DB)
    → EJS (render template with data)
    → Puppeteer (headless Chrome → A4 PDF)
    → Response (PDF download)
    → Cleanup (delete temp files)
```

---

## 🔐 Security

- **Password hashing**: bcryptjs with salt rounds = 10 (Mongoose pre-save hook)
- **Authentication**: JWT with 1-hour expiry, Bearer scheme
- **Protected routes**: Auth middleware on `/documents` and `/documents/history`
- **DB injection protection**: Mongoose schema typing prevents query injection
- **XSS prevention**: EJS escaped output (`<%= %>`), React JSX auto-escaping
- **Secrets management**: All keys in `.env`, gitignored
- **No CSRF**: Token in `Authorization` header (not cookies)

---

## 🚢 Deployment

### Backend (Render.com)

1. Create a new **Web Service** on Render
2. Connect your GitHub repo
3. Set:
   - **Build Command**: `npm install`
   - **Start Command**: `npm start`
4. Add all environment variables from `.env` in Render dashboard
5. Deploy — Puppeteer Chrome binary is downloaded via `postinstall` script

### Frontend (Vercel)

1. Import your GitHub repo in Vercel
2. Set:
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
3. Deploy — Vercel auto-detects Vite

> Update `frontend/src/api.js` `baseURL` to point to your deployed backend URL.

---

## 🧪 Testing

> ⚠️ Tests are not yet implemented. The project currently relies on manual testing for auth flows, document generation, chatbot filtering, and consultation booking.

**Planned test strategy:**
- **Backend**: Jest + Supertest + mongodb-memory-server for route-level integration tests
- **Frontend**: React Testing Library for form submission flows (mocked Axios)

---

## 🛣 Roadmap

- [ ] **Redis caching** for compiled EJS templates and user sessions
- [ ] **PDF generation queue** (Bull + Redis) for async document processing
- [ ] **Refresh token flow** with httpOnly cookies
- [ ] **Zod validation schemas** replacing manual field checks
- [ ] **Database pagination** on document history endpoint
- [ ] **Rate limiting** on auth and chatbot endpoints
- [ ] **OAuth login** (Google, GitHub)
- [ ] **Docker Compose** for one-command local setup
- [ ] **Unit + integration tests** with CI pipeline

---

## 📝 License

This project is for portfolio and learning purposes.
