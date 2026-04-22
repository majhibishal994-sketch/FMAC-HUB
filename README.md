# ⚡ FMAC Hub
### Fakir Mohan Autonomous College — Events & Notices Platform

A Gen-Z styled campus hub for **FM Autonomous College, Baleshwar, Odisha** — built for students to track events, notices, and volunteer opportunities.

---

## 📁 Project Structure

```
fmac/
├── backend/              ← Node.js + Express + MongoDB
│   ├── models/
│   │   ├── User.js       ← Admin user model
│   │   ├── Event.js      ← Events model
│   │   └── Notice.js     ← Notices model
│   ├── routes/
│   │   ├── auth.js       ← Login / Signup routes
│   │   ├── events.js     ← CRUD + volunteer email
│   │   ├── notices.js    ← CRUD for notices
│   │   └── admin.js      ← Approve/reject admins
│   ├── middleware/
│   │   └── auth.js       ← JWT auth middleware
│   ├── server.js         ← Entry point
│   ├── package.json
│   └── .env.example      ← Copy to .env and fill in
│
└── frontend/             ← React app
    ├── public/
    │   └── index.html
    └── src/
        ├── context/
        │   └── AuthContext.js
        ├── utils/
        │   └── api.js
        ├── components/
        │   ├── Navbar.js + .css
        ├── pages/
        │   ├── Home.js + .css
        │   ├── Events.js + .css
        │   ├── EventDetail.js + .css
        │   ├── Notices.js + .css
        │   ├── Login.js
        │   ├── Signup.js
        │   ├── Auth.css
        │   └── AdminDashboard.js + .css
        ├── App.js
        ├── App.css
        └── index.js
```

---

## 🚀 Setup Guide

### Prerequisites
- Node.js v18+
- MongoDB Atlas account (free tier works)
- Gmail account (for optional email features)

---

### Step 1: MongoDB Atlas Setup

1. Go to [mongodb.com/atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Create a database user with a password
4. Under **Network Access**, add `0.0.0.0/0` (allow from anywhere)
5. Click **Connect → Drivers** and copy the connection string
   - It looks like: `mongodb+srv://username:password@cluster.mongodb.net/`

---

### Step 2: Backend Setup

```bash
cd backend
npm install
cp .env.example .env
```

Edit `.env`:
```env
PORT=5000
MONGODB_URI=mongodb+srv://YOUR_USER:YOUR_PASS@cluster.mongodb.net/fmac_db?retryWrites=true&w=majority
JWT_SECRET=some_very_long_random_secret_string_here
CLIENT_URL=http://localhost:3000
```

Start the backend:
```bash
npm run dev    # development (with nodemon)
# or
npm start      # production
```

The server will auto-create the **default admin** on first run:
- **Name:** Bishal Majhi
- **Email:** umakantamajhi195@gmail.com
- **Password:** Bishal@123

---

### Step 3: Frontend Setup

```bash
cd frontend
npm install
```

Create `.env` in frontend folder:
```env
REACT_APP_API_URL=http://localhost:5000/api
```

Start the frontend:
```bash
npm start
```

App opens at [http://localhost:3000](http://localhost:3000)

---

## 🌐 Deploy Online

### Option A: Railway (Recommended — Free)

**Backend:**
1. Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub
2. Upload/push your `backend/` folder
3. Set environment variables in Railway dashboard:
   - `MONGODB_URI`, `JWT_SECRET`, `CLIENT_URL` (your Vercel frontend URL)
4. Railway gives you a URL like: `https://fmac-backend.up.railway.app`

**Frontend:**
1. Go to [vercel.com](https://vercel.com) → New Project → Import frontend folder
2. Set environment variable:
   - `REACT_APP_API_URL=https://your-backend.up.railway.app/api`
3. Deploy → get URL like `https://fmac-hub.vercel.app`
4. Update `CLIENT_URL` in Railway with the Vercel URL

---

### Option B: Render (Free)

**Backend on Render:**
1. [render.com](https://render.com) → New Web Service
2. Connect GitHub repo, set root to `backend/`
3. Build command: `npm install`
4. Start command: `npm start`
5. Add env vars

**Frontend on Vercel or Netlify:**
- Same as above, set `REACT_APP_API_URL` to your Render backend URL

---

### Option C: Self-host (VPS)

```bash
# Install PM2 globally
npm install -g pm2

# Backend
cd backend
npm install
pm2 start server.js --name fmac-backend

# Frontend (build first)
cd frontend
npm install
npm run build
# Serve build/ folder with nginx or serve
npm install -g serve
serve -s build -l 3000
```

---

## ✨ Features

| Feature | Details |
|---|---|
| 🏠 Home Page | Hero section, upcoming events, latest notices |
| 🎯 Events Page | Grid layout with search by keywords |
| 🔍 Search | Search events by title, description, tags, venue |
| 📅 Date Filter | Filter events between a date range |
| 🏷️ Category Filter | Filter by cultural, technical, sports, academic, etc. |
| 📄 Event Detail | Full event info, volunteer button → opens email |
| 📋 Notices | PDF notice board with category filter |
| 📥 PDF Download | Click any notice to download/open its PDF |
| 🙋 Volunteering | Click "Apply to Volunteer" → pre-filled mailto opens |
| 🔐 Admin Login | Only approved admins can log in |
| 📝 Admin Signup | Submit college ID for verification |
| ✅ Admin Approval | Current admins can approve/reject new admin requests |
| ➕ Add Event | Admin form with banner upload, tags, dates, etc. |
| ➕ Add Notice | Admin form with PDF upload, importance flag |
| 🗑️ Delete | Admins can delete events and notices |
| 👥 Manage Admins | View, remove admins from dashboard |

---

## 🔑 Default Admin Credentials

| Field | Value |
|---|---|
| Name | Bishal Majhi |
| Email | umakantamajhi195@gmail.com |
| Password | Bishal@123 |

> ⚠️ Change the password after first login via your MongoDB Atlas dashboard.

---

## 📡 API Endpoints

### Auth
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/signup` | Submit admin request |
| POST | `/api/auth/login` | Admin login |
| GET | `/api/auth/me` | Get current user |

### Events (Public)
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/events` | List events (search, filter, paginate) |
| GET | `/api/events/:id` | Get event details |
| POST | `/api/events/:id/volunteer` | Get mailto data for volunteering |

### Events (Admin)
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/events` | Create event |
| PUT | `/api/events/:id` | Update event |
| DELETE | `/api/events/:id` | Delete event |

### Notices (Public)
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/notices` | List notices |
| GET | `/api/notices/:id` | Get notice |

### Notices (Admin)
| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/notices` | Create notice |
| DELETE | `/api/notices/:id` | Delete notice |

### Admin Management
| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/admin/pending` | Get pending requests |
| GET | `/api/admin/admins` | Get all admins |
| POST | `/api/admin/approve/:id` | Approve admin |
| POST | `/api/admin/reject/:id` | Reject admin |
| DELETE | `/api/admin/remove/:id` | Remove admin |

---

## 🛠️ Tech Stack

**Backend:** Node.js, Express, MongoDB/Mongoose, JWT, bcryptjs, Multer, Nodemailer

**Frontend:** React 18, React Router v6, Axios, date-fns, react-hot-toast

**Fonts:** Syne (display) + DM Sans (body)

---

## 📱 Mobile Responsive
Fully responsive — works on mobile, tablet, and desktop.

---

*Built for FM Autonomous College students. Not an official college website.*
