# MEAN Stack CRUD Application - DevOps Deployment

This repository contains the complete containerization and CI/CD setup for a full-stack MEAN (MongoDB, Express, Angular, Node.js) application, deployed on an AWS EC2 instance.

## 🚀 Live Demo
- **URL:** [http://13.232.140.134/](http://13.232.140.134/)
- **API Health Check:** [http://13.232.140.134/api/tutorials](http://13.232.140.134/api/tutorials)

---

## 🛠️ Tech Stack & Infrastructure
- **Frontend:** Angular 15
- **Backend:** Node.js & Express
- **Database:** MongoDB (Containerized)
- **Proxy:** Nginx (Reverse Proxy on Port 80)
- **Containerization:** Docker & Docker Compose
- **CI/CD:** GitHub Actions
- **Cloud:** AWS EC2 (Ubuntu 22.04)

---

## 📦 Containerization Details

### Multi-Stage Dockerfiles
- **Frontend:** Uses a multi-stage build. Stage 1 builds the Angular app using `node:18-alpine`, and Stage 2 serves the static files using `nginx:alpine`.
- **Backend:** Optimized Node.js environment using `node:18-alpine`.

### Docker Compose Orchestration
The `docker-compose.yml` file manages four services:
1. `mongo`: Official MongoDB image with persistent volume mapping.
2. `backend`: The Node.js API connected to Mongo.
3. `frontend`: The Angular application.
4. `nginx`: Acts as the gateway, routing `/api` to the backend and `/` to the frontend.

---

## 🔄 CI/CD Pipeline (GitHub Actions)
The pipeline is defined in `.github/workflows/main.yml` and triggers on every push to the `main` branch.

**Pipeline Stages:**
1. **Build & Push:** 
   - Authenticates with Docker Hub.
   - Builds fresh Docker images for both frontend and backend.
   - Pushes images to Docker Hub (`abhinandan069/`).
2. **Automated Deployment:**
   - Connects to the EC2 instance via SSH.
   - Pulls the latest images from Docker Hub.
   - Restarts containers using `docker-compose`.

---

## ⚙️ Setup & Installation

### Local Development
1. Clone the repo.
2. Run `docker-compose up --build`.
3. Access at `http://localhost`.

### EC2 Deployment
1. Ensure Docker and Docker Compose are installed on the VM.
2. Create a directory `~/mean-app` and copy `docker-compose.yml` and `nginx.conf`.
3. Set up GitHub Secrets:
   - `DOCKER_USERNAME` / `DOCKER_PASSWORD`
   - `EC2_HOST`: `13.232.140.134`
   - `EC2_SSH_KEY`: Content of your `.pem` file.
4. Push to `main` to trigger the auto-deploy.

---

## 📄 Infrastructure Details (Nginx Configuration)
Nginx is configured as a reverse proxy to handle all traffic on Port 80.
- `location /api`: Proxy pass to `http://backend:3000`
- `location /`: Proxy pass to `http://frontend:80`

---
*Submitted for Discover Dollar DevOps Internship Assignment.*
