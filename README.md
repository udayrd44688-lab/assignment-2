In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## Project setup

### Node.js Server

cd backend

npm install

You can update the MongoDB credentials by modifying the `db.config.js` file located in `app/config/`.

Run `node server.js`

### Angular Client

cd frontend

npm install

Run `ng serve --port 8081`

You can modify the `src/app/services/tutorial.service.ts` file to adjust how the frontend interacts with the backend.

Navigate to `http://localhost:8081/`

## Docker (Local)

From the project root:

```bash
docker compose up -d --build
```

The app will be available at `http://localhost/` (port 80).

## Production Deployment (VM)

1. Provision an Ubuntu VM and open inbound port 80.
2. Install Docker and Docker Compose on the VM.
3. Clone this repository onto the VM (e.g. `~/crud-dd-task-mean-app`).
4. Create a `.env` file in the repo root from `.env.example` and set your Docker Hub username.
5. Start the stack using images from Docker Hub:

```bash
docker compose -f docker-compose.prod.yml up -d
```

## CI/CD (GitHub Actions)

The workflow at `.github/workflows/ci-cd.yml`:
- Builds and pushes Docker images on every push to `main`.
- SSHes into the VM and runs `docker compose pull` + `up -d`.

Required GitHub secrets:
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `VM_HOST`
- `VM_USER`
- `VM_SSH_KEY`
- `VM_APP_PATH` (path to the repo on the VM, e.g. `/home/ubuntu/crud-dd-task-mean-app`)
