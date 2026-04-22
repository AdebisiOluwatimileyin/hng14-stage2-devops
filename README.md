# HNG14 Stage 2 — Containerized Microservices

A job processing system made up of three services: a frontend, an API, and a worker, all containerized with Docker and orchestrated with Docker Compose.

## Services

- **Frontend** (Node.js/Express) — Port 3000: Submit and track jobs
- **API** (Python/FastAPI) — Port 8000: Creates jobs and serves status updates
- **Worker** (Python) — Processes jobs from the queue
- **Redis** — Shared message queue between API and worker

## Prerequisites

- Docker
- Docker Compose

## How to Run Locally

**1. Clone the repository:**
```bash
git clone https://github.com/AdebisiOluwatimileyin/hng14-stage2-devops.git
cd hng14-stage2-devops
```

**2. Copy environment variables:**
```bash
cp .env.example .env
```

**3. Build and start all services:**
```bash
docker compose up --build
```

**4. Verify services are running:**
```bash
docker compose ps
```

A successful startup looks like:


## Live Deployment
http://54.174.111.132

## Endpoints

### Frontend (Port 3000)
- `POST /submit` — Submit a new job
- `GET /status/:id` — Get job status

### API (Port 8000)
- `GET /health` — Health check
- `POST /jobs` — Create a new job
- `GET /jobs/:id` — Get job status

## CI/CD Pipeline

GitHub Actions pipeline runs in this order:
1. Lint — flake8, eslint, hadolint
2. Test — pytest with coverage report
3. Build — builds and pushes images to local registry
4. Security Scan — Trivy scans all images
5. Integration Test — brings full stack up and tests job flow
6. Deploy — rolling update on push to main