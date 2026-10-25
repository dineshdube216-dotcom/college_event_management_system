# College Event Management System (CEMS) — MVP (30% Milestone)

Centralized, role-based cloud web application for college event announcements. Official university events are published by admins, club events are proposed by presidents and verified by mentors, and students browse everything in a single verified feed with highlighted official announcements.

---

## 🚀 Quick Start Guide

### Prerequisites
- Node.js v18+ and npm installed

### 1. Install & Setup Database

```bash
# Navigate to backend and install packages
cd backend
npm install

# Push Prisma schema to SQLite / PostgreSQL database
npx prisma db push

# Seed sample events & default test users for all 4 roles
npm run seed
```

### 2. Run Backend & Frontend Servers

**Terminal 1 (Backend API - Port 5000):**
```bash
cd backend
npm run dev
```

**Terminal 2 (Frontend React App - Port 5173):**
```bash
cd frontend
npm install
npm run dev
```

Open your browser at **`http://localhost:5173`**.

---

## 🔑 Default Test Accounts

Use these pre-seeded accounts to test role functionality:

| Role | Email | Password | Role Description |
| :--- | :--- | :--- | :--- |
| **Student** | `student@college.edu` | `Password123!` | Verified student account, registers for events |
| **President** | `president@college.edu` | `Password123!` | Club President account (Coding Club) |
| **Mentor** | `mentor@college.edu` | `Password123!` | Faculty mentor overseeing student clubs |
| **Admin** | `admin@college.edu` | `Password123!` | Dean / University Administration |

*(Or click the Quick Demo buttons on the Login page!)*

---

## ⚙️ Environment Variables

The backend uses environment variables stored in `backend/.env`:

```env
PORT=5000
DATABASE_URL="file:./dev.db" # SQLite local dev; set to "postgresql://user:pass@host:5432/dbname" for production
JWT_SECRET="cems_super_secret_jwt_key_2026_secure"
CLIENT_URL="http://localhost:5173"

# Optional Nodemailer SMTP setup (Leave blank for dev console logging & instant dev OTP banners!)
SMTP_HOST="smtp.ethereal.email"
SMTP_PORT=587
SMTP_USER=""
SMTP_PASS=""
SMTP_FROM="CEMS Notifications <noreply@cems.college.edu>"
```

---

## 🎯 End-to-End Demo Workflow

1. **Sign Up**: Go to `/signup`, fill in student details (photo, name, email, ADT number, phone, department, password).
2. **OTP Email Verification**: Upon submission, a 6-digit OTP code is generated. In dev mode, the OTP code is displayed in a dev banner on screen & printed in the server console for instant verification.
3. **Verify OTP**: Enter the 6 digits on `/verify-otp`. Upon verification, an authenticated JWT session is created and you are redirected to the feed.
4. **Browse Student Feed**: View the grid of seeded events. Note how **Official Events** are visually highlighted with amber gold badges (`#F59E0B`), star icons, and distinct borders compared to **Club Events**.
5. **Filters & Search**: Use the top filter chips (`All`, `Official Events`, `Club Events`) and search bar to filter by keyword or venue.
6. **Register**: Click "Register" on any event to register your student account.
7. **Forgot / Reset Password**: Test `/forgot-password` to send a reset link, then open `/reset-password?token=...` to set a new password.

---

## 📁 Database Schema Summary

Implemented in `backend/prisma/schema.prisma`:
- `Users` (user_id, name, email, password_hash, role, department, adt_no, phone_no, photo_url, email_verified, created_at)
- `Clubs` (club_id, club_name, mentor_id FK, president_id FK)
- `Events` (event_id, title, description, created_by FK, club_id FK, status, event_type, event_date, venue, banner_image_url, registration_link, is_highlighted, created_at)
- `Event_Approvals` (approval_id, event_id FK, mentor_id FK, status, remarks, reviewed_at)
- `Registrations` (registration_id, event_id FK, student_id FK, registered_at)
- `OTP_Verifications` (otp_id, user_id FK, otp_code, purpose, expires_at, is_used)
- `Notifications` (notification_id, user_id FK, event_id FK, message, sent_at, channel)
