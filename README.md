# Pathway — Internship Opportunity Portal

A React and Express application for students to discover and apply for internships, and for administrators to manage opportunities and application outcomes. MongoDB stores all accounts and portal records; the dashboard statistics are queried from the database.

## Requirements

- Node.js 18 or newer
- MongoDB Community Server or a MongoDB Atlas connection string
- npm

## Setup

1. Start MongoDB locally, or create a database in MongoDB Atlas.
2. Copy `.env.example` to `backend/.env`.
3. Set `MONGODB_URI` and replace `JWT_SECRET` with a random value of at least 32 characters. Before creating an admin account, also set a private `ADMIN_EMAIL` and an `ADMIN_PASSWORD` of at least 12 characters.
4. Install and start the API:

   ```powershell
   cd backend
   npm install
   npm run dev
   ```

5. In another terminal, install and start the web application:

   ```powershell
   cd frontend
   npm install
   npm run dev
   ```

6. Open [http://localhost:5173](http://localhost:5173). The API listens on [http://localhost:5000](http://localhost:5000).
7. Create the first admin account from the backend folder:

   ```powershell
   npm run seed:admin
   ```

   Seeding is safe to repeat: if that email already exists, the script leaves the account unchanged. The admin credentials are read from the environment, never embedded in source.

## Main features

- Student registration and sign-in using an HTTP-only JWT cookie, profile editing, internship search, saved opportunities, applications, and status history.
- Administrator dashboard with live MongoDB counts, opportunity create/edit/publish/delete, student directory, and application search/status management.
- Role-protected Express routes, bcrypt password hashing, input validation, duplicate-application prevention through a unique database index, centralized API errors, Helmet, and rate-limited authentication endpoints.
- Responsive layouts for desktop and mobile. No internship catalogue or dashboard statistics are hardcoded.

## API outline

- `POST /api/auth/register`, `POST /api/auth/login`, `POST /api/auth/logout`, `GET /api/auth/me`, `PATCH /api/auth/profile`
- `GET /api/internships`, `GET /api/internships/:id`
- Admin: `GET /api/internships/admin/all`, `POST /api/internships`, `PATCH /api/internships/:id`, `DELETE /api/internships/:id`
- Student: `POST /api/applications/internships/:internshipId`, `GET /api/applications/mine`
- Admin: `GET /api/applications`, `PATCH /api/applications/:id/status`
- Student bookmarks: `GET /api/bookmarks`, `POST /api/bookmarks/:internshipId`, `DELETE /api/bookmarks/:internshipId`
- Dashboards: `GET /api/dashboard/student`, `GET /api/dashboard/admin`, admin student listing `GET /api/dashboard/students`

The Vite development server proxies same-origin `/api` requests to `http://localhost:5000`, keeping cookie-based authentication reliable on both `localhost` and `127.0.0.1`. If deploying the frontend and API separately, set `VITE_API_URL` to the API's base URL and `CLIENT_ORIGIN` to the exact frontend origin. Keep them on the same site (for example, sibling subdomains) so the HTTP-only `SameSite=Lax` session cookie works; use HTTPS in production so secure cookies are enabled.

## Build

```powershell
cd frontend
npm run build
```
