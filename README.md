# 🚀 Production-Ready CI/CD Pipeline with Docker, AWS EC2 and GitHub Actions

## 📖 Overview

This project demonstrates the complete journey of taking a simple Node.js application from local development to a production-like deployment environment using Docker, AWS EC2, Nginx, Docker Hub, and GitHub Actions.

The goal of this project was not just to deploy an application, but to understand the complete DevOps workflow:

```text
Code → GitHub → Docker → Docker Hub → AWS EC2 → Nginx → Live Application
```

and later automate the process using CI/CD:

```text
Code Push → GitHub Actions → Docker Hub → EC2 Deployment
```

---

# 🏗 Project Architecture

## Before CI/CD

```text
Developer
    │
    ▼
Node.js Application
    │
    ▼
Docker Image
    │
    ▼
Docker Hub
    │
    ▼
AWS EC2
    │
    ▼
Nginx Reverse Proxy
    │
    ▼
Users
```

## After CI/CD

```text
Git Push
   │
   ▼
GitHub Actions
   │
   ├── Build Docker Image
   ├── Push to Docker Hub
   └── Deploy to EC2
          │
          ▼
       Nginx
          │
          ▼
       Live App
```

---

# ⚙️ Tech Stack

* Node.js
* Express.js
* Docker
* Docker Hub
* Git & GitHub
* GitHub Actions
* AWS EC2
* Ubuntu Linux
* Nginx
* Bash Scripting

---

# 🚀 Project Journey

## Phase 1: Application Development

### Objective

Build a simple application that can later be containerized and deployed.

### Tasks Performed

Created a Node.js application using Express.

Implemented two routes:

```javascript
/
```

Returns application response.

```javascript
/health
```

Returns application health status.

Configured server:

```javascript
app.listen(PORT, "0.0.0.0")
```

### Why 0.0.0.0?

Containerized applications must listen on all interfaces.

If localhost is used:

```text
Container starts
But application is inaccessible from outside
```

This small configuration becomes important during Docker deployment.

---

# Phase 2: Version Control with GitHub

### Objective

Store and manage source code.

### Tasks Performed

Initialized Git repository:

```bash
git init
```

Created:

```text
.gitignore
```

Ignored:

```text
node_modules/
.env
```

Committed code:

```bash
git add .
git commit -m "Initial Node.js app setup"
```

Pushed to GitHub repository.

### Why GitHub?

* Source code management
* Collaboration
* Version history
* CI/CD integration

---

# Phase 3: Containerization using Docker

### Objective

Package application and dependencies into a portable container.

### Dockerfile

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

### Image Build

```bash
docker build -t devops-app .
```

### Run Container

```bash
docker run -d -p 3000:3000 devops-app
```

### Concepts Learned

#### Docker Image

Blueprint of the application.

#### Docker Container

Running instance of an image.

#### Port Mapping

```bash
-p 3000:3000
```

Meaning:

```text
Host Port → Container Port
```

### Verification

```bash
docker ps
docker logs <container_id>
```

---

# Phase 4: Docker Hub Integration

### Objective

Store container images in a central registry.

### Tagging Image

```bash
docker tag devops-app <username>/devops-app:latest
```

### Push Image

```bash
docker push <username>/devops-app:latest
```

### Pull Image

```bash
docker pull <username>/devops-app:latest
```

### Why Docker Hub?

Acts as artifact storage.

Benefits:

* Versioned images
* Easy distribution
* CI/CD integration
* Central repository

---

# Phase 5: AWS EC2 Deployment

### Objective

Deploy application on a real cloud server.

### Tasks Performed

Created:

* Ubuntu EC2 Instance

Configured Security Groups:

* SSH (22)
* HTTP (80)
* Application Port (3000)

Connected using SSH:

```bash
ssh -i key.pem ubuntu@<public-ip>
```

Installed Docker:

```bash
sudo apt update
sudo apt install docker.io -y
```

Pulled image:

```bash
docker pull <username>/devops-app:latest
```

Started container:

```bash
docker run -d -p 3000:3000 <username>/devops-app:latest
```

### Result

Application became accessible publicly through EC2 public IP.

---

# Phase 6: Nginx Reverse Proxy

### Objective

Expose application on standard HTTP port.

### Problem

Without Nginx:

```text
http://server-ip:3000
```

### Solution

Install Nginx:

```bash
sudo apt install nginx -y
```

Configure reverse proxy:

```nginx
location / {
    proxy_pass http://localhost:3000;
}
```

### Request Flow

```text
User
  ↓
Nginx (80)
  ↓
Node App (3000)
```

### Result

Application became accessible via:

```text
http://server-ip
```

without specifying port.

---

# Phase 7: CI/CD using GitHub Actions

### Objective

Automate Docker image build and push process.

### Workflow File

```text
.github/workflows/deploy.yml
```

### Workflow Tasks

1. Checkout repository
2. Login to Docker Hub
3. Build Docker image
4. Push image to Docker Hub

### GitHub Secrets Used

```text
DOCKER_USERNAME
DOCKER_PASSWORD
```

### Outcome

Whenever code is pushed:

```text
Git Push
   ↓
GitHub Actions
   ↓
Docker Build
   ↓
Docker Hub Push
```

No manual image creation required.

---

# 🔍 Key DevOps Concepts Practiced

* Source Code Management
* Containerization
* Image Management
* Container Registry
* Linux Server Administration
* Cloud Deployment
* Reverse Proxy Configuration
* CI/CD Pipelines
* GitHub Actions
* Docker Networking
* SSH Connectivity
* Deployment Automation

---

# 🎯 Interview Questions I Should Be Able To Answer

### Why Docker?

To ensure consistent application behavior across environments.

### Difference between Image and Container?

Image → Blueprint

Container → Running instance

### Why Docker Hub?

Stores and distributes Docker images.

### Why AWS EC2?

Provides cloud infrastructure to host applications.

### Why Nginx?

Acts as reverse proxy and exposes applications through standard web ports.

### Why use 0.0.0.0?

Allows the application to listen on all network interfaces.

### What is CI/CD?

Automating build, test and deployment processes.

### What happens when you push code?

GitHub Actions triggers workflow → Builds Docker image → Pushes image to Docker Hub.

### Explain the complete flow of your project.

```text
Developer
   ↓
GitHub
   ↓
GitHub Actions
   ↓
Docker Build
   ↓
Docker Hub
   ↓
AWS EC2
   ↓
Docker Container
   ↓
Nginx
   ↓
Users
```

---

# 📈 Future Improvements

* Automatic deployment to EC2 after image push
* HTTPS using SSL certificates
* Custom domain integration
* Monitoring and logging
* Infrastructure as Code using Terraform
* Kubernetes deployment

---

# 🏆 Key Outcome

Successfully built and deployed a containerized Node.js application using Docker, AWS EC2 and Nginx, and automated the build and image delivery process using GitHub Actions, gaining hands-on experience with real-world DevOps workflows and deployment practices.
