In this DevOps task, you need to build and deploy a full-stack CRUD application using the MEAN stack (MongoDB, Express, Angular 15, and Node.js). The backend will be developed with Node.js and Express to provide REST APIs, connecting to a MongoDB database. The frontend will be an Angular application utilizing HTTPClient for communication.  

The application will manage a collection of tutorials, where each tutorial includes an ID, title, description, and published status. Users will be able to create, retrieve, update, and delete tutorials. Additionally, a search box will allow users to find tutorials by title.

## 🔗 Deployed Application
The application is deployed and can be accessed at: [https://s-poo.github.io/crud-dd-task-mean-app/](https://s-poo.github.io/crud-dd-task-mean-app/) (Note: Deployment link might be updated soon)


## Containerization & Deployment (Docker)

This application is containerized using Docker and orchestrated with Docker Compose.

### Local Deployment

1.  **Prerequisites**: Ensure Docker and Docker Compose are installed.
2.  **Build and Start**:
    ```bash
    docker-compose up --build
    ```
3.  **Access the Application**:
    - Frontend: `http://localhost:80` (Proxied via Nginx)
    - Backend API: `http://localhost:80/api`
    - MongoDB: `mongodb://localhost:27017`

### Docker Configuration
- **Backend**: Uses `node:18-alpine`. Database URL is configurable via `MONGODB_URL` environment variable.
- **Frontend**: Uses a multi-stage build. Stage 1 builds the Angular app; Stage 2 serves it using Nginx.
- **Nginx Infrastructure**: Acts as a reverse-proxy on port 80.
  - Routes `/api/` to the Backend container (`http://backend:8080/api/`).
  - Routes all other traffic to the Frontend container (`http://frontend:80`).
  - Includes `mime.types` for proper static asset serving.

## CI/CD Pipeline (GitHub Actions)

A GitHub Actions workflow is provided in `.github/workflows/deploy.yml`.

### Workflow Steps
1.  **Build**: Automatically builds Docker images for frontend and backend on every push to the `main` branch.
2.  **Push**: Pushes the built images to Docker Hub.
3.  **Deploy**: Connects to the target Ubuntu VM via SSH, pulls the latest images, and restarts the containers using Docker Compose.

### Required Secrets
To use the pipeline, configure the following secrets in your GitHub repository:
- `DOCKERHUB_USERNAME`: Your Docker Hub username.
- `DOCKERHUB_TOKEN`: Your Docker Hub access token.
- `VM_HOST`: The IP address or hostname of your Ubuntu VM.
- `VM_USERNAME`: The SSH username for the VM.
- `VM_SSH_KEY`: Your private SSH key for authentication.

## Screenshots

As part of the deliverables, the following screenshots are required to showcase the successful implementation and deployment:

### 1. CI/CD Configuration & Execution
The CI/CD pipeline is defined in `.github/workflows/deploy.yml`. It automates the build, push, and deployment process.

### 2. Docker Image Build & Push
Images are built for both frontend and backend and pushed to Docker Hub as `mean-frontend` and `mean-backend`.

### 3. Application Deployment & Working UI
![Working UI](/C:/Users/pooja/.gemini/antigravity/brain/f3bef8bb-1db6-492a-b5a9-8e0651831696/tutorials_list_ui_1771923783919.png)
*(The screenshot above shows the application running with the 'Tutorials List' visible)*

## Setup and Deployment Instructions

### Option 1: Docker Compose (Preferred)
1.  **Start Services**:
    ```bash
    docker-compose up --build
    ```
2.  **Access**:
    - App: `http://localhost:80`
    - API: `http://localhost:80/api/`

### Option 2: Manual Setup (Development)

**1. MongoDB**
Ensure MongoDB is running locally on port 27017.

**2. Node.js Backend**
1. `cd backend`
2. `npm install`
3. `node server.js` (Running on port 8080)

**3. Angular Frontend**
1. `cd frontend`
2. `npm install`
3. `npm run start -- --port 8081 --proxy-config src/proxy.conf.json`
4. Access App at `http://localhost:8081`
