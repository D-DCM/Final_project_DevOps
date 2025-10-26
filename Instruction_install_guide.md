
---

## 🧩 Step-by-Step Implementation

### 1️⃣ Build the Flask App
A simple "Hello World" Flask application (`app.py`):
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello World from Flask CI/CD!"

2️⃣ Create Docker Image
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]

docker build -t hello-flask .
docker tag hello-flask <your_dockerhub_username>/final_project_flask:latest
docker push <your_dockerhub_username>/final_project_flask:latest

3️⃣ Deploy to Kubernetes
apiVersion: apps/v1
kind: Deployment
metadata:
  name: final-project
spec:
  replicas: 2
  selector:
    matchLabels:
      app: final-project
  template:
    metadata:
      labels:
        app: final-project
    spec:
      containers:
      - name: final-project
        image: <your_dockerhub_username>/final_project_flask:latest
        ports:
        - containerPort: 5000

service.yaml
apiVersion: v1
kind: Service
metadata:
  name: final-project-service
spec:
  type: NodePort
  selector:
    app: final-project
  ports:
    - port: 5000
      targetPort: 5000
      nodePort: 30001

run locally:
kubectl apply -f k8s/
kubectl get pods
kubectl get svc
http://localhost:30001/

4️⃣ Deploy Using Helm

Helm Chart: final-project-chart/
Contains:

Chart.yaml

values.yaml

templates/deployment.yaml

templates/service.yaml

Install:

helm install final-project final-project-chart/


Upgrade:

helm upgrade final-project final-project-chart/

5️⃣ CI/CD Automation with GitHub Actions

Workflow file: .github/workflows/helm-deploy.yaml

name: Helm Deploy to Kubernetes

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Login to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build & Push Docker image
      run: |
        docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/final_project_flask:${{ github.sha }} .
        docker push ${{ secrets.DOCKERHUB_USERNAME }}/final_project_flask:${{ github.sha }}

    # - name: Deploy with Helm
    #   run: |
    #     echo "Helm deployment skipped (local cluster only)"


Secrets used:

DOCKERHUB_USERNAME

DOCKERHUB_TOKEN

KUBE_CONFIG (optional for remote clusters)

✅ Results

Flask app deployed successfully via Kubernetes.

Docker image pushed automatically to Docker Hub.

Helm manages the deployment configuration.

GitHub Actions automates build and deploy on each push.

.

👤 Author

Omer Raviv
DevOps Engineer & Technician @ Verifone
Driven by technology, automation, and growth.


