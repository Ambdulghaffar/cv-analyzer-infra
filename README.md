# CV Analyzer AI — Infrastructure

This repository orchestrates the full CV Analyzer AI project (frontend + backend) using Docker Compose, for demonstration and DevOps learning purposes.

## About this repository

This repository does not contain any application code itself. Instead, it references two independent repositories as Git submodules — [cv-analyzer-web](https://github.com/Ambdulghaffar/cv-analyzer-web) (frontend) and [cv-analyzer-api](https://github.com/Ambdulghaffar/cv-analyzer-api) (backend) — and provides the Docker Compose configuration needed to run them together with a single command.

## Project structure

```
cv-analyzer-infra/
├── cv-analyzer-web/    (submodule — Next.js frontend)
├── cv-analyzer-api/    (submodule — FastAPI backend)
├── docker-compose.yml
└── README.md
```

## Architecture overview

```
                        docker compose up
                                │
                ┌───────────────┴────────────────┐
                │                                 │
        ┌───────▼────────┐               ┌────────▼───────┐
        │  web container  │               │  api container  │
        │  Next.js        │               │  FastAPI        │
        │  port 3000      │               │  port 8000      │
        └───────┬────────┘               └────────┬───────┘
                │                                 │
                └───────────── Docker network ─────┘

        User's browser
                │
                ├── http://localhost:3000  ───► web container
                └── http://localhost:8000  ───► api container
```

> **Note:** although both containers share an internal Docker network, the frontend calls the backend directly from the **user's browser** (via `NEXT_PUBLIC_API_URL`, set to `http://localhost:8000`), not from within the container itself. The Docker network is therefore not used for browser-to-API calls.

## Prerequisites

- Docker and Docker Compose installed
- Git (required to fetch the submodules)

## Getting started

1. Clone this repository together with its submodules:
   ```bash
   git clone --recurse-submodules <URL of this repo>
   ```

2. If you already cloned it without the submodules option, initialize them separately:
   ```bash
   git submodule update --init --recursive
   ```

3. Create a `.env` file inside `cv-analyzer-web/` with the required variables:
   ```
   NEXT_PUBLIC_SUPABASE_URL=
   NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=
   NEXT_PUBLIC_API_URL=http://localhost:8000
   ```
   The purpose of each variable is documented in the [cv-analyzer-web README](https://github.com/Ambdulghaffar/cv-analyzer-web).

4. Create a `.env` file inside `cv-analyzer-api/` with the required variables:
   ```
   SUPABASE_URL=
   SUPABASE_SERVICE_ROLE_KEY=
   GROQ_API_KEY=
   CORS_ORIGINS=http://localhost:3000
   ```
   The purpose of each variable is documented in the [cv-analyzer-api README](https://github.com/Ambdulghaffar/cv-analyzer-api).

5. From the root of this repository, build and start both services:
   ```bash
   docker compose up --build
   ```

6. Access the applications:
   - Frontend: [http://localhost:3000](http://localhost:3000)
   - Interactive API documentation: [http://localhost:8000/docs](http://localhost:8000/docs)

## Updating submodules

This repository does not update its submodules automatically — each one is pinned to a specific commit. To update a submodule to its latest commit:

```bash
cd cv-analyzer-web
git pull origin main
```

Then, from the root of this repository, commit the new submodule reference:

```bash
git add cv-analyzer-web
git commit -m "chore: update web submodule"
git push
```

The same process applies to `cv-analyzer-api`.

## Related repositories

- [cv-analyzer-web](https://github.com/Ambdulghaffar/cv-analyzer-web) — Next.js frontend
- [cv-analyzer-api](https://github.com/Ambdulghaffar/cv-analyzer-api) — FastAPI backend

## Author

**Ambdulghaffar Ahamadi**

- GitHub: [github.com/ambdulghaffar](https://github.com/ambdulghaffar)
- LinkedIn: [linkedin.com/in/ambdulghaffar-ahamadi](https://www.linkedin.com/in/ambdulghaffar-ahamadi-7a476839a/)
