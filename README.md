# CV Analyzer AI — Infrastructure

This repository orchestrates the full CV Analyzer AI project (frontend + backend) using Docker Compose, for demonstration and DevOps learning purposes.

## About the project

CV Analyzer AI is a fullstack platform that uses AI (Llama 3.3 70B via Groq) to analyze the compatibility between a resume and a job posting. It offers two distinct experiences:

- **Candidate mode** — upload a resume and a job description to get a weighted compatibility score, a detailed breakdown (required/preferred skills, experience, education, presentation), concrete improvement suggestions, and an AI-generated cover letter (with tone and language selection, exportable to PDF/Word).
- **Recruiter mode** — compare up to 10 candidates against a single job posting and get them automatically ranked by compatibility score, manage a library of saved job offers, and review past ranking sessions.

Both modes share the same scoring engine, built on a carefully designed prompt (chain-of-thought reasoning, anti-hallucination rules, structured JSON output validated with Pydantic) and a weighted scoring model (35% required skills, 10% preferred skills, 30% experience, 15% education, 10% presentation quality).

## About this repository

This repository does not contain any application code itself. Instead, it references two independent repositories as Git submodules — [cv-analyzer-web](https://github.com/Ambdulghaffar/cv-analyzer-web) (frontend) and [cv-analyzer-api](https://github.com/Ambdulghaffar/cv-analyzer-api) (backend) — and provides the Docker Compose configuration needed to run them together with a single command.

## Features at a glance

| | Candidate | Recruiter |
|---|---|---|
| AI-powered analysis | ✅ Single resume vs. job posting | ✅ Multiple resumes vs. job posting, ranked |
| Score breakdown | ✅ Skills, experience, education, presentation | ✅ Same, per candidate |
| History | ✅ Past analyses | ✅ Past ranking sessions |
| Saved job offers | — | ✅ Full CRUD |
| Cover letter generation | ✅ Tone + language selection, PDF/Word export | — |
| Authentication | Email/password + Google OAuth, role-based onboarding | Same |

## Tech stack

- **Frontend:** Next.js 15 (App Router), TypeScript, Tailwind CSS, shadcn/ui
- **Backend:** FastAPI (Python), Groq API (Llama 3.3 70B), pdfplumber
- **Database & Auth:** Supabase (PostgreSQL, Auth with JWKS-based JWT verification)
- **Infrastructure:** Docker, Docker Compose, Git submodules

## Project structure
cv-analyzer-infra/
├── cv-analyzer-web/    (submodule — Next.js frontend)
├── cv-analyzer-api/    (submodule — FastAPI backend)
├── docker-compose.yml
└── README.md

## Architecture overview
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

> **Note:** although both containers share an internal Docker network (used for server-side calls from the frontend, e.g. dashboard overview pages), the browser calls the backend directly (via `NEXT_PUBLIC_API_URL`, set to `http://localhost:8000`), not through the container.

## Prerequisites

- Docker and Docker Compose installed
- Git (required to fetch the submodules)
- A Supabase project and a Groq API key (see each sub-project's README for details)

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
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=
NEXT_PUBLIC_API_URL=http://localhost:8000
INTERNAL_API_URL=http://api:8000
NEXT_PUBLIC_SITE_URL=http://localhost:3000
   The purpose of each variable is documented in the [cv-analyzer-web README](https://github.com/Ambdulghaffar/cv-analyzer-web).

4. Create a `.env` file inside `cv-analyzer-api/` with the required variables:
SUPABASE_URL=
SUPABASE_SERVICE_ROLE_KEY=
GROQ_API_KEY=
CORS_ORIGINS=http://localhost:3000
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

## Author

**Ambdulghaffar Ahamadi**

- Portfolio: [ambdulghaffar-portfolio.vercel.app](https://ambdulghaffar-portfolio.vercel.app)
- GitHub: [github.com/ambdulghaffar](https://github.com/ambdulghaffar)
- LinkedIn: [linkedin.com/in/ambdulghaffar-ahamadi](https://www.linkedin.com/in/ambdulghaffar-ahamadi-7a476839a/)
- Email: [elhaffarahamadi@gmail.com](mailto:elhaffarahamadi@gmail.com)