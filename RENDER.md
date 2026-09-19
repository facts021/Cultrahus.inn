# Connecting & Deploying Cultrahus Sangam 2026 to Render

This application is fully pre-configured for deployment on [Render](https://render.com) as a Node.js Web Service.

---

## Option 1: Automatic 1-Click Blueprint (Recommended)

Render detects the included `render.yaml` specification automatically:

1. Push or export this repository to your **GitHub** or **GitLab** account.
2. Go to your [Render Dashboard](https://dashboard.render.com).
3. Click **New +** → **Blueprint**.
4. Connect your repository. Render will automatically read `render.yaml` and configure:
   - **Service Name:** `cultrahus-sangam-2026`
   - **Environment:** `Node`
   - **Build Command:** `npm run render-build` (installs dependencies including build tools and compiles client & server)
   - **Start Command:** `npm start` (launches `dist/server.cjs`)
   - **Health Check Path:** `/api/health`
5. Click **Apply**. Render will build and deploy your application live!

---

## Option 2: Manual Web Service Setup

If you prefer setting up via the Render Web Service form:

1. In Render Dashboard, click **New +** → **Web Service**.
2. Select your repository.
3. Configure the settings:
   - **Name:** `cultrahus-sangam-2026`
   - **Region:** Any (e.g., Oregon, Frankfurt, Singapore)
   - **Branch:** `main` (or your default branch)
   - **Root Directory:** *(leave blank / root)*
   - **Runtime:** `Node`
   - **Build Command:** `npm run render-build` *(or `npm install --include=dev && npm run build`)*
   - **Start Command:** `npm start`
   - **Instance Type:** `Free` (or Starter)
4. Under **Advanced**:
   - **Health Check Path:** `/api/health`
   - **Auto-Deploy:** `Yes`
5. **Environment Variables** (Optional / Pre-configured):
   - `NODE_ENV`: `production`
   - `PORT`: `10000` (Render's default)
6. Click **Deploy Web Service**.

---

## Technical Notes

- **Client & Server Compilation:** The build command executes `vite build` to bundle the React frontend into `dist/`, and uses `esbuild` to compile `server.ts` into a standalone CommonJS bundle at `dist/server.cjs`.
- **Port Handling:** On Render (`RENDER=true`), the server binds to Render's assigned `PORT` (or default `10000`) and `0.0.0.0`. In the local dev container, it remains pinned to `3000`.
- **Firebase Firestore Integration:** Firebase client credentials (`firebase-applet-config.json`) are bundled at build time, ensuring registration and ticket passes sync seamlessly with your Firestore project (`cultrahus`).
- **Data Persistence:** The local database (`data/sangam_records.json`) runs automatically. If you use a paid Render plan with a Persistent Disk, mount your disk to `/var/data` and set the environment variable `DATA_DIR=/var/data`.
