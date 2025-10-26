# Final Project - DevOps CI/CD Pipeline (Git → Docker → Kubernetes → Helm)

## 👨‍💻 Project Overview
This project demonstrates a complete CI/CD pipeline that automates the process of building, testing, and deploying a Flask web application using:
- **GitHub** (Version Control)
- **Docker** (Containerization)
- **Kubernetes** (Orchestration)
- **Helm** (Deployment Management)
- **GitHub Actions** (CI/CD Automation)

---

## 🚀 Project Architecture



1. **GitHub Actions** builds and pushes the Docker image on each push to `main`.
2. **Docker Hub** stores the built image.
3. **Helm** automates deployment to Kubernetes.
4. **Kubernetes (Docker Desktop)** runs the pods and services locally.

---

## 🧱 Project Structure
Final_project_DevOps/
│
├── app/
│ └── app.py
│
├── k8s/
│ ├── deployment.yaml
│ └── service.yaml
│
├── final-project-chart/
│ ├── Chart.yaml
│ ├── values.yaml
│ └── templates/
│ ├── deployment.yaml
│ ├── service.yaml
│ └── _helpers.tpl
│
├── .github/
│ └── workflows/
│ └── helm-deploy.yaml
│
└── Dockerfile