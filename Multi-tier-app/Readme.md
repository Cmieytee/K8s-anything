# 🏗️ Multi-Tier Kubernetes Application (Minikube)

This repository demonstrates how to build and deploy a **multi-tier application** using **Kubernetes on Minikube**, featuring:

- 🌐 **Frontend:** React
- 🧠 **Backend:** Python Flask
- 💾 **Database:** PostgreSQL

The app is split into three layers to promote **separation of concerns**, **scalability**, **security**, and **maintainability**.

---

## 🧱 Architecture Overview

A multi-tier application is structured in layers:

- **Presentation Tier (Frontend)** – React  
- **Application Tier (Backend)** – Flask (Python)  
- **Data Tier (Database)** – PostgreSQL  

---

## 🗂️ Project Structure


multi-tier-app/
├── k8s/
│   ├── namespace.yaml (kubectl create ns)
│   ├── db-deployment.yaml
│   ├── db-service.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-service.yaml
│   └── ingress.yaml
├── backend/
│   └── Dockerfile
├── frontend/
│   └── Dockerfile
└── README.md


---

## 🚀 Getting Started

### ✅ Prerequisites

- Docker
- Minikube
- Kubectl
- Node.js & npm

---

## 🛠️ Setup Instructions

### 1. Start Minikube and Enable Ingress

```bash
minikube start
minikube addons enable ingress

### 2. Create the Frontend

# Remove existing broken folder
cd /Users/marymedoye/Personal-dev/K8s-anything/Multi-tier-app
rm -rf frontend

# Recreate and scaffold React app
mkdir frontend
cd frontend
npx create-react-app .

# For local dev:
npm start

# Build production bundle:
npm run build

Then create a Dockerfile inside frontend/:

### 3. Build Docker Images Inside Minikube

eval $(minikube docker-env)
docker build -t multi-tier-backend:latest ./backend
docker build -t multi-tier-frontend:latest ./frontend

Ensure imagePullPolicy: Never is set in both deployment YAMLs.

### 4. Apply Kubernetes Resources

kubectl apply -n multi-tier -f K8s-manifest/

🌐 Testing the Application

minikube service frontend-service --url -n multi-tier
minikube service backend-service --url -n multi-tier

Test:

React App: http://127.0.0.1:<frontend-port>/

Backend API: http://127.0.0.1:<backend-port>/api/hello