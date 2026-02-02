# Netflix Clone

A Netflix-style movie/TV browsing demo with a React + Vite frontend and an Express + MongoDB backend. The app fetches movie and TV data from The Movie Database (TMDB) and provides user authentication, search, watch/trailer pages, and a responsive UI built with Tailwind CSS.

This README was created by inspecting the repository and filled with the actual stack, important env vars, and run instructions.

## Table of Contents

- [Demo](#demo)
- [Features](#features)
- [Tech stack](#tech-stack)
- [Repository layout](#repository-layout)
- [Prerequisites](#prerequisites)
- [Environment variables](#environment-variables)
- [Local development (run frontend & backend)](#local-development-run-frontend--backend)
- [Running a production build](#running-a-production-build)
- [Common troubleshooting](#common-troubleshooting)
- [Contributing](#contributing)
- [License & contact](#license--contact)

## Demo

No public demo URL provided. You can run locally (instructions below).

## Features

- Responsive Netflix-style UI
- Browse trending movies / TV shows and category sliders
- Search movies/TV/people and view search history
- Watch/trailer page with embedded player
- User authentication using JWT stored in an httpOnly cookie
- Backend middlewares for protected routes
- Uses TMDB API for content data and TMDB image endpoints for covers/posters

## Tech stack

- Frontend
  - React (JSX) with Vite
  - Tailwind CSS
  - React Router
  - Zustand (state)
  - axios for HTTP
  - react-hot-toast, lucide-react, react-player, etc.
- Backend
  - Node.js + Express
  - MongoDB via Mongoose
  - JWT authentication (token set in httpOnly cookie)
  - dotenv, cookie-parser, axios (for TMDB requests)
- External APIs
  - The Movie Database (TMDB) — backend uses a Bearer token to call TMDB
- Dev tooling
  - Vite for frontend
  - ESLint config present for frontend

## Repository layout (important folders)

- frontend/ — React + Vite app
  - src/ — React components, pages, store, utils
  - vite.config.js, index.html
- backend/ — Express server
  - index.js — server entry
  - config/ — envVars and DB connect
  - controllers/, routes/, services/, middlewares/, models/
- README.md — this file

## Prerequisites

- Node.js (v16+ recommended)
- npm or yarn
- MongoDB (atlas or local) for persistent data
- TMDB API token (v4 Bearer token recommended) — used by backend

## Environment variables

Create a `.env` file in the backend folder (backend/.env). Minimum variables used by the backend:

```
MONGO_URI=<your_mongodb_connection_string>
PORT=5000
JWT_SECRET=<a_long_random_secret_for_jwt>
TMDB_API_KEY=<your_tmdb_bearer_token_or_api_key>
NODE_ENV=development
```

Notes:
- The backend uses `Authorization: Bearer <TMDB_API_KEY>` when calling TMDB (see backend/services/tmdb.service.js). Use the TMDB v4 read access token (starts with `Bearer ...`) or a proper API token per TMDB docs.
- `MONGO_URI` can be a MongoDB Atlas connection string or a local mongodb://... URL.
- If you add any frontend-specific env vars (e.g., VITE_API_URL), put them in `frontend/.env` using the `VITE_` prefix (Vite requirement).

## Local development (run frontend & backend)

Open two terminals (one for backend, one for frontend).

1. Clone the repository and install top-level deps if any:
   git clone https://github.com/MostafaWahid/NetflixClone.git
   cd NetflixClone

2. Backend
   cd backend
   npm install
   # create backend/.env (see env vars above)
   # Start backend
   npm run dev
   # If `dev` is not defined, try:
   # node index.js
   # or
   # npx nodemon index.js

   The backend listens on the PORT from env (default 5000). API base path is `/api/v1/*`.

3. Frontend
   cd ../frontend
   npm install
   npm run dev

   Vite dev server runs (default port 5173). The frontend currently makes axios calls to relative `/api/v1/...` endpoints. When frontend and backend are on different ports in development, do one of the following:
   - Add a proxy in Vite config to forward `/api` to the backend (recommended), e.g. in `frontend/vite.config.js`:
     ```
     // add server.proxy: { '/api': 'http://localhost:5000' }
     ```
   - Or set an env variable in `frontend/.env` (e.g. `VITE_API_URL=http://localhost:5000`) and set axios baseURL at app start:
     ```js
     // frontend/src/main.jsx (example)
     import axios from 'axios'
     axios.defaults.baseURL = import.meta.env.VITE_API_URL || ''
     ```
   - Alternatively, build the frontend and serve the static files from the backend (production flow).

4. Open the frontend URL printed by Vite (usually http://localhost:5173) and sign up / log in to use protected routes.




