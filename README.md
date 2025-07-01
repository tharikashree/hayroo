# 🛍️ Hayroo - MERN Stack eCommerce Website

A full-featured eCommerce platform built with the **MERN stack** (MongoDB, Express.js, React, Node.js) and deployed using **Docker**, **Kubernetes**, and **Jenkins CI/CD** pipeline.

---

## 📦 Tech Stack

| Layer           | Technology            |
|----------------|------------------------|
| Frontend       | React.js               |
| Backend        | Node.js + Express.js   |
| Database       | MongoDB                |
| CI/CD          | Jenkins                |
| Containerization | Docker               |
| Orchestration  | Kubernetes (K8s)       |
| Cloud-ready    | Supports Minikube / any K8s cluster |

---

## 🚀 Features

- 🛒 Add to cart, checkout, and order summary
- 🔐 User authentication
- 💳 Simulated payment gateway
- 📦 Admin product management
- 📊 Order & transaction tracking
- ☁️ CI/CD via Jenkins pipelines
- 🐳 Dockerized microservices
- ☸️ Kubernetes deployment (client + server)

---

## 📁 Project Structure

    hayroo/
    ├── client/ # React frontend
    ├── server/ # Node.js backend
    ├── k8s/ # Kubernetes manifests
    │ ├── client-deployment.yaml
    │ ├── client-service.yaml
    │ ├── server-deployment.yaml
    │ └── server-service.yaml
    ├── Jenkinsfile # CI/CD pipeline
    └── README.md


---

## ⚙️ Setup Instructions

### 🧱 Prerequisites

- Docker & Docker Compose
- Minikube or Kubernetes Cluster
- Jenkins (configured with Docker access)
- MongoDB instance (can be deployed via K8s or Atlas)

---

## 🧪 Local Development

### 1. Clone the Repository

```bash
  git clone https://github.com/tharikashree/hayroo.git
  cd client
  npm start
  cd server
  npm start
```

### 2. Build and Run (Docker Only)

    # Build images
    docker build -t tharikashree/client ./client
    docker build -t tharikashree/server ./server
    
    # Run containers
    docker run -p 3000:3000 tharikashree/client
    docker run -p 5000:5000 tharikashree/server

## Jenkins CI/CD Pipeline
The project includes a declarative Jenkinsfile for automated build, push, and deploy.

Pipeline Stages:
  1.Clone GitHub repo
  
  2.Login to Docker Hub
  
  3.Build Docker images
  
  4.Push images to Docker Hub
  
  5.Apply Kubernetes manifests

Ensure Jenkins is configured with:
    
    dockerhub-creds-id → Docker Hub username + access token
    kubeconfig-secret → Kube config file (for cluster access)

Kubernetes Deployment
1. Start Minikube (optional)

        minikube start
2. Apply K8s Manifests

        kubectl apply -f k8s/
3. Access Services

        # Check service URLs
        kubectl get svc

# If using Minikube
      minikube service client-service
      minikube service server-service
Environment Variables (example)
Set up .env files for both client and server:

server/.env

    MONGO_URI=mongodb://localhost:27017/hayroo
    PORT=5000
    JWT_SECRET=your_jwt_secret
client/.env

    REACT_APP_API_BASE_URL=http://localhost:5000/api

📄 License
This project is licensed under the MIT License.



