# Deployment Guide: Render/Railway (Backend) & Vercel (Frontend)

This guide explains how to deploy the Stellar Bounty Board backend to Render or Railway, and the frontend to Vercel. It includes required environment variables, health check paths, and troubleshooting tips.

---

## Backend Deployment: Render

### 1. Create a New Web Service
- Go to [Render](https://render.com/).
- Click **New Web Service** and connect your fork of this repo.
- Select the `backend/` directory as the root.
- Set the build command: `npm install && npm run build`
- Set the start command: `npm start`

### 2. Environment Variables
- (If needed) Add any required environment variables. By default, none are strictly required for local JSON persistence.
- If you add secrets (e.g., for future Postgres or API keys), add them here.

### 3. Health Check Path
- Set health check path to: `/api/health`
- A healthy response is `{ "service": "stellar-bounty-board-backend", "status": "ok", ... }`

### 4. Root Directory
- Use `backend/` as the root directory for the Render service.

### 5. Troubleshooting
- If deploy fails, check logs for missing dependencies or build errors.
- Ensure the port is set to `3001` (or use `process.env.PORT` as Render provides).

---

## Backend Deployment: Railway (Alternative)

Railway provides a seamless way to deploy Node.js applications with a `railway.toml` configuration.

### 1. Connect Your Repo
- Go to [Railway](https://railway.app/).
- Click **New Project** -> **Deploy from GitHub repo**.
- Select your fork of the repository.

### 2. Configuration
- Railway will automatically detect the `railway.toml` in the repository root.
- It will use the `backend/` directory to build and start the service as defined in the config.

### 3. Environment Variables
- Add the required environment variables (listed below) in the **Variables** tab of your Railway service.
- Railway automatically provides a `PORT` variable.

### 4. Health Check
- The `railway.toml` already configures the health check path: `/api/health`.
- Railway will wait for this path to return a 200 OK before marking the deployment as healthy.

---

## Frontend Deployment: Vercel

### 1. Import Project
- Go to [Vercel](https://vercel.com/).
- Click **New Project** and import your fork of this repo.
- Set the root directory to `frontend/`.

### 2. Environment Variables
- If your backend is public, set `VITE_API_BASE_URL` to your backend URL (e.g., `https://your-backend.up.railway.app/api` or `https://your-backend.onrender.com/api`).
- Example:
  - `VITE_API_BASE_URL=https://your-backend.up.railway.app/api`

### 3. Build & Output Settings
- Build command: `npm run build`
- Output directory: `dist`

---

## Required Environment Variables

### Backend (Render & Railway)
- `PORT`: (Optional) Port the server listens on. Defaults to `3001` (Railway/Render provide this automatically).
- `GITHUB_WEBHOOK_SECRET`: (Required if using webhooks) Secret to verify GitHub webhook signatures.
- `BOUNTY_STORE_PATH`: (Optional) Path to the JSON file for bounty data. Defaults to `./data/bounties.json`.
- `BOUNTY_AUDIT_STORE_PATH`: (Optional) Path to the JSON file for audit logs.

### Frontend (Vercel)
- `VITE_API_BASE_URL` (Required): URL of your deployed backend API (e.g., `https://your-backend.up.railway.app/api`).

---

## Health Check Paths
- Backend: `/api/health` (should return status `ok`)
- Frontend: `/` (should load the React dashboard)

---

## Common Deployment Issues & Fixes
- **Build fails:** Check Node.js version (18+), install all dependencies, and verify build commands.
- **API not reachable:** Confirm backend is live and CORS is configured.
- **Env vars not set:** Double-check environment variable names and values in Render/Railway/Vercel dashboards.
- **Frontend shows blank:** Ensure correct output directory (`dist`) and that the API URL is set.

---

## Need Help?
- Check the [ONBOARDING.md](../ONBOARDING.md) for local setup.
- Open an issue or discussion in the repo for deployment help.
