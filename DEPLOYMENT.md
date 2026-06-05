# StockFlow Deployment Guide

## Prerequisites
- Docker Hub account
- GitHub account
- Accounts on Render (backend) and Vercel (frontend)

---

## 1. Push to GitHub

```bash
cd /path/to/stockflow
git init
git add .
git commit -m "feat: production-ready StockFlow v2"
git remote add origin https://github.com/<your-username>/stockflow.git
git push -u origin main
```

---

## 2. Push Backend Image to Docker Hub

```bash
# Build and tag
docker build -t <your-dockerhub-username>/stockflow-backend:latest ./backend

# Log in and push
docker login
docker push <your-dockerhub-username>/stockflow-backend:latest
```

---

## 3. Deploy Backend on Render

1. Go to [render.com](https://render.com) → **New → Web Service**
2. Connect your GitHub repo, select the `stockflow` repository
3. Set **Root Directory** to `backend`
4. Set **Build Command**: `pip install -r requirements.txt`
5. Set **Start Command**: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
6. Add **Environment Variables**:

| Variable | Value |
|---|---|
| `DATABASE_URL` | Your PostgreSQL connection string (see step 3a) |
| `CORS_ORIGINS` | Your Vercel frontend URL (e.g., `https://stockflow.vercel.app`) |

### 3a. Create PostgreSQL on Render
- **New → PostgreSQL** → copy the **External Database URL**
- Use this as your `DATABASE_URL`

---

## 4. Deploy Frontend on Vercel

1. Go to [vercel.com](https://vercel.com) → **New Project**
2. Import your GitHub repo → set **Root Directory** to `frontend`
3. Add **Environment Variable**:

| Variable | Value |
|---|---|
| `VITE_API_URL` | Your Render backend URL (e.g., `https://stockflow-api.onrender.com`) |

4. Click **Deploy** — Vercel auto-detects Vite.

---

## 5. Verify

- Frontend: `https://your-app.vercel.app`
- Backend API: `https://your-api.onrender.com/health`
- Swagger Docs: `https://your-api.onrender.com/docs`

---

## Local Development (Docker Compose)

```bash
docker compose up --build
```

| Service | URL |
|---|---|
| Frontend | http://localhost:5173 |
| Backend API | http://localhost:8000 |
| Swagger Docs | http://localhost:8000/docs |
