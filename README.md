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
        cd hayroo

# Build images
docker build -t tharikashree/client ./client
docker build -t tharikashree/server ./server

# Run containers (example)
docker run -p 3000:3000 tharikashree/client
docker run -p 5000:5000 tharikashree/server
