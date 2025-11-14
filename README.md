**Project Overview**
- **Name**: Verdict AI — a two-part app with a Node backend and a Vite + React (TypeScript) frontend.
- **Structure**: backend API in `backend/` and frontend app in `verdict-react/`.

**Quick Summary**
- **Backend**: small Node server at `backend/server.js` (install with `npm install` in `backend`).
- **Frontend**: Vite + React app in `verdict-react/` (use `npm install` and `npm run dev`).

**Prerequisites**
- **Node.js**: v18+ recommended. Confirm with `node -v`.
- **npm** (bundled with Node) or **pnpm/yarn** if you prefer.
- Optional: **bun** (project includes `bun.lockb` if you prefer `bun install`).
- Optional: **Supabase CLI** if you want to run migrations locally.

**Repository Layout**
- **`backend/`**: Node API server.
  - `server.js` — main server file
  - `package.json` — dependencies + scripts for backend
- **`verdict-react/`**: frontend app (Vite + React + TypeScript)
  - `src/` — React source files
  - `public/` — static files (includes `pdf.worker.min.js`)
  - `package.json` — frontend scripts and deps
- **`supabase/`**: Supabase config & migrations

**Environment Variables**
The project uses `.env` files in both `backend/` and `verdict-react/`. Do NOT commit secrets.

Suggested examples (create or update `backend/.env`):
```
# backend/.env (example)
PORT=3001
SUPABASE_URL=https://your-project-ref.supabase.co
SUPABASE_SERVICE_KEY=your-supabase-service-key-or-secret
OPENAI_API_KEY=sk-...    # or GEMINI_API_KEY if using Google Gemini
OTHER_SECRET=...
```

Suggested examples (create or update `verdict-react/.env`):
```
# verdict-react/.env (Vite requires `VITE_` prefix for variables exposed to client)
VITE_SUPABASE_URL=https://your-project-ref.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
VITE_API_URL=http://localhost:3001   # points to backend while developing
```

How to find required variables
- Check `backend/server.js` and files under `verdict-react/src/utils/` for any explicit environment names required by the code.

Installation & Local Development (Windows PowerShell)

1) Start the backend

Open PowerShell and run:
```powershell
cd .\backend
npm install
# If `package.json` has a start/dev script use it (preferable):
npm run dev    # or `npm start` if defined
# Fallback: run the server directly
node server.js
```

2) Start the frontend (Vite)

In a new PowerShell window/tab:
```powershell
cd .\verdict-react
npm install
npm run dev
# Vite will print the local URL (usually http://localhost:5173)
```

Notes for `bun` users
- If you prefer `bun`:
```powershell
cd .\verdict-react
bun install
bun run dev   # or `bun dev` depending on your setup
```

Running both together
- Start the backend first (so `VITE_API_URL` can reach it), then start the frontend.

Production / Build
- Build the frontend for production:
```powershell
cd .\verdict-react
npm run build
# Serve the `dist/` directory with any static host or use `npm run preview` (if defined)
```
- Backend: use your usual Node process manager (PM2, systemd, Docker). Example PM2:
```powershell
cd .\backend
npm install -g pm2
pm2 start server.js --name verdict-backend
```

Supabase migrations
- The repo contains `supabase/migrations/` SQL. If you use the Supabase CLI you can push migrations:
```powershell
supabase db reset      # WARNING: destructive on local DB
supabase db push       # push schema to local or remote DB depending on your config
```
Check the Supabase docs and your `supabase/config.toml` for target database settings.

API / CORS
- By default the frontend communicates with backend via `VITE_API_URL`.
- If you run frontend on a different origin, ensure the backend enables CORS for the frontend origin.

Troubleshooting & Common Issues
- Port already in use: change `backend/.env` `PORT` or stop the process occupying the port.
- Missing env keys: server will fail if required keys are missing — re-check `.env` files.
- CORS errors: add the frontend origin to backend CORS config or set CORS to `*` for local dev only.
- PDF worker issues: frontend includes `public/pdf.worker.min.js` — ensure it's served from `public/` so PDF viewer libraries can load it.
- Supabase auth/keys: ensure you use the correct key type (`anon` for client, service role for server-side operations).

Security
- Never commit `.env` files or secrets to git. Add them to `.gitignore` (there are `.gitignore` files in repo root and subfolders already).

Contribution
- Create feature branches, run linters and formatters used by the repo, and open PRs against `main`.

Useful Commands Summary
- Backend install & start (PowerShell):
```powershell
cd .\backend; npm install; npm run dev
```
- Frontend install & start (PowerShell):
```powershell
cd .\verdict-react; npm install; npm run dev
```
- Build frontend:
```powershell
cd .\verdict-react; npm run build
```

Where to look next
- `backend/server.js` — server entry point and list of required env variables
- `verdict-react/src/` — React app sources and any client-side env usage
- `supabase/migrations/` — DB migration SQL

If anything is missing in these instructions (scripts, exact env names), tell me and I will update the `README.md` with exact commands and env templates after checking `package.json` and `server.js`.
