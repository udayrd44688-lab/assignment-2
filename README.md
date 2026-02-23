# MEAN CRUD App - DevOps Deployment

This project is a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, Node.js). The backend exposes REST APIs, the frontend consumes them via HTTPClient, and Nginx serves the SPA while reverse-proxying `/api` to the backend.

The application manages a collection of tutorials with ID, title, description, and published status. Users can create, retrieve, update, delete, and search tutorials by title.

**Repository Structure**
- `backend/`: Node.js + Express API with MongoDB
- `frontend/`: Angular 15 UI
- `docker-compose.yml`: Local Docker stack
- `docker-compose.prod.yml`: Production stack (Docker Hub images)
- `.github/workflows/ci-cd.yml`: CI/CD pipeline

## Prerequisites
- Docker Desktop or Docker Engine
- GitHub account and repository access
- Docker Hub account
- Ubuntu VM with port 80 open
- SSH key access to the VM

## Local Development (No Docker)

1. Backend:
   ```bash
   cd backend
   npm install
   node server.js
   ```
2. Frontend:
   ```bash
   cd frontend
   npm install
   npm start
   ```
3. Open `http://localhost:8081/`.

MongoDB connection can be changed in `backend/app/config/db.config.js`.

## Docker (Local)

From the project root:
```bash
docker compose up -d --build
```

The app will be available at `http://localhost/` (port 80).

## Docker Hub Images (Manual)

If you want to build and push manually:
```bash
docker login
docker build -t <dockerhub-username>/mean-backend:latest ./backend
docker build -t <dockerhub-username>/mean-frontend:latest ./frontend
docker push <dockerhub-username>/mean-backend:latest
docker push <dockerhub-username>/mean-frontend:latest
```

## Production Deployment (Ubuntu VM)

1. Provision an Ubuntu VM and open inbound port 80.
2. Install Docker and Docker Compose.
3. Clone this repository on the VM.
4. Create `.env` in the repo root from `.env.example` and set `DOCKERHUB_USERNAME`.
5. Start the stack:
   ```bash
   docker compose -f docker-compose.prod.yml up -d
   ```

The production stack uses:
- `mongo:6` for the database
- Docker Hub images for backend and frontend

## Nginx Reverse Proxy

Nginx is built into the frontend container and configured in `frontend/nginx.conf`.
- Static Angular files are served at `/`
- API requests on `/api/*` are proxied to the backend service (`backend:8080`)
- The host maps port 80 to the frontend container via `docker-compose.yml` and `docker-compose.prod.yml`

## CI/CD (GitHub Actions)

The workflow in `.github/workflows/ci-cd.yml`:
- Builds and pushes Docker images on every push to `main`
- SSHes into the VM and runs `docker compose pull` + `docker compose up -d`

Required GitHub secrets:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `VM_HOST`
- `VM_USER`
- `VM_SSH_KEY`
- `VM_APP_PATH` (e.g. `/home/ubuntu/crud-dd-task-mean-app`)

## Screenshots (Required Deliverables)

Place screenshots in `docs/screenshots/` with the following filenames:
- `docs/screenshots/cicd-workflow.png` (CI/CD configuration page)
- `docs/screenshots/cicd-run.png` (workflow execution showing build/push)
- `docs/screenshots/dockerhub-images.png` (Docker Hub images/tags)
- `docs/screenshots/app-ui.png` (running UI in browser)
- `docs/screenshots/nginx-infra.png` (Nginx config or infra diagram)
