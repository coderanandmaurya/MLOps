# Kubernetes for ML Deployment with FastAPI (Linear Regression Example)

This guide covers:

1. ML model training
2. FastAPI inference API
3. Dockerization
4. Kubernetes deployment
5. Scaling and production concepts
6. Full architecture
7. Folder structure
8. Commands step-by-step

---

# 1. High Level Architecture

```text
                ┌──────────────────┐
                │     Client       │
                │ (Web/Postman)    │
                └────────┬─────────┘
                         │ HTTP Request
                         ▼
                ┌──────────────────┐
                │ Kubernetes       │
                │ Service          │
                │ (Load Balancer)  │
                └────────┬─────────┘
                         │
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
   ┌──────────┐   ┌──────────┐   ┌──────────┐
   │ FastAPI  │   │ FastAPI  │   │ FastAPI  │
   │ Pod 1    │   │ Pod 2    │   │ Pod 3    │
   └────┬─────┘   └────┬─────┘   └────┬─────┘
        │              │              │
        └──────────────┼──────────────┘
                       │
                ┌──────────────┐
                │ model.pkl    │
                │ Linear Reg   │
                └──────────────┘
```

---

# 2. Project Structure

```text
ml-k8s-project/
│
├── app/
│   ├── main.py
│   ├── model.pkl
│   └── train.py
│
├── k8s/
│   ├── deployment.yml
│   └── service.yml
│
├── Dockerfile
├── requirements.txt
└── README.md
```

---

# 3. Train Linear Regression Model

## `train.py`

```python
from sklearn.linear_model import LinearRegression
import numpy as np
import pickle

# Sample Data
X = np.array([[1], [2], [3], [4], [5]])
y = np.array([2, 4, 6, 8, 10])

# Train Model
model = LinearRegression()
model.fit(X, y)

# Save Model
with open("model.pkl", "wb") as f:
    pickle.dump(model, f)

print("Model Saved")
```

---

# 4. FastAPI Application

## Install FastAPI

```bash
pip install fastapi uvicorn scikit-learn numpy
```

---

## `main.py`

```python
from fastapi import FastAPI
from pydantic import BaseModel
import pickle
import numpy as np

app = FastAPI()

# Load Model
with open("model.pkl", "rb") as f:
    model = pickle.load(f)

# Request Schema
class InputData(BaseModel):
    value: float

@app.get("/")
def home():
    return {"message": "ML API Running"}

@app.post("/predict")
def predict(data: InputData):

    prediction = model.predict(
        np.array([[data.value]])
    )

    return {
        "input": data.value,
        "prediction": float(prediction[0])
    }
```

---

# 5. Run Locally

## Train model

```bash
python train.py
```

## Start FastAPI

```bash
uvicorn main:app --reload
```

Open:

```text
http://127.0.0.1:8000/docs
```

Swagger UI appears automatically.

---

# 6. Test API

## POST Request

```json
{
  "value": 10
}
```

## Response

```json
{
  "input": 10,
  "prediction": 20.0
}
```

---

# 7. Dockerize the Application

## Docker Architecture

```text
Application
   │
   ▼
Docker Container
   │
   ▼
Portable ML Service
```

---

## `requirements.txt`

```text
fastapi
uvicorn
scikit-learn
numpy
pydantic
```

---

## Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app/ .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# 8. Build Docker Image

## Build Image

```bash
docker build -t ml-fastapi .
```

---

## Run Container

```bash
docker run -p 8000:8000 ml-fastapi
```

---

# 9. Kubernetes Concepts

| Component          | Purpose                   |
| ------------------ | ------------------------- |
| Pod                | Runs container            |
| Deployment         | Manages replicas          |
| Service            | Exposes API               |
| ReplicaSet         | Maintains pods            |
| Horizontal Scaling | Adds more pods            |
| ConfigMap          | Stores configs            |
| Secret             | Stores passwords/API keys |

---

# 10. Kubernetes Architecture

```text
                    Kubernetes Cluster
┌─────────────────────────────────────────────────┐
│                                                 │
│   ┌──────────────────────────────────────┐      │
│   │ Deployment                           │      │
│   │ replicas: 3                          │      │
│   └──────────────┬───────────────────────┘      │
│                  │                              │
│      ┌───────────┼───────────┐                  │
│      ▼           ▼           ▼                  │
│   ┌──────┐   ┌──────┐   ┌──────┐                │
│   │ Pod1 │   │ Pod2 │   │ Pod3 │                │
│   └──────┘   └──────┘   └──────┘                │
│         \       |       /                       │
│          \      |      /                        │
│           ▼     ▼     ▼                         │
│         Kubernetes Service                      │
│                Load Balancing                   │
└─────────────────────────────────────────────────┘
```

---

# 11. Install Kubernetes

Use:

* Docker Desktop Kubernetes
* Minikube
* Kind

Recommended for beginners:

* Minikube
* kubectl

Official sites:

* [Kubernetes Documentation](https://kubernetes.io/docs/home/?utm_source=chatgpt.com)
* [DL for kubernetes copy to powershell](curl.exe -LO "https://dl.k8s.io/release/v1.36.0/bin/windows/amd64/kubectl.exe")
* [Minikube](https://minikube.sigs.k8s.io/docs/?utm_source=chatgpt.com)

---

# 12. Start Minikube

```bash
minikube start
```

Check:

```bash
kubectl get nodes
```

---

# 13. Kubernetes Deployment File

## `deployment.yml`

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: ml-fastapi-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: ml-fastapi

  template:
    metadata:
      labels:
        app: ml-fastapi

    spec:
      containers:
      - name: ml-fastapi-container
        image: ml-fastapi:latest

        imagePullPolicy: Never

        ports:
        - containerPort: 8000
```

---

# 14. Kubernetes Service File

## `service.yml`

```yaml
apiVersion: v1
kind: Service

metadata:
  name: ml-fastapi-service

spec:
  selector:
    app: ml-fastapi

  ports:
    - protocol: TCP
      port: 80
      targetPort: 8000

  type: NodePort
```

---

# 15. Apply Kubernetes Files

## Create Deployment

```bash
kubectl apply -f k8s/deployment.yml
```

## Create Service

```bash
kubectl apply -f k8s/service.yml
```

---

# 16. Check Running Resources

## Pods

```bash
kubectl get pods
```

## Services

```bash
kubectl get svc
```

## Deployments

```bash
kubectl get deployments
```

---

# 17. Access Application

If using Minikube:

```bash
minikube service ml-fastapi-service
```

---

# 18. Scaling in Kubernetes

## Increase Pods

```bash
kubectl scale deployment ml-fastapi-deployment --replicas=5
```

---

# 19. Auto Scaling (HPA)

```bash
kubectl autoscale deployment ml-fastapi-deployment \
--cpu-percent=50 \
--min=2 \
--max=10
```

---

# 20. Real Production ML Architecture

```text
                    User Request
                          │
                          ▼
                   API Gateway
                          │
                          ▼
                    Load Balancer
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
     FastAPI Pod     FastAPI Pod     FastAPI Pod
          │               │               │
          └───────────────┼───────────────┘
                          │
                    Model Storage
                 (S3 / MLflow / PVC)
                          │
                          ▼
                    Monitoring Stack
           (Prometheus + Grafana + Loki)
```

---

# 21. Important Kubernetes Commands

| Command                         | Purpose       |
| ------------------------------- | ------------- |
| `kubectl get pods`              | View pods     |
| `kubectl logs pod-name`         | View logs     |
| `kubectl describe pod pod-name` | Debug pod     |
| `kubectl delete pod pod-name`   | Delete pod    |
| `kubectl apply -f file.yml`     | Apply config  |
| `kubectl get svc`               | View services |

---

# 22. Production Improvements

## Use These Tools

| Tool       | Purpose        |
| ---------- | -------------- |
| MLflow     | Model registry |
| Prometheus | Metrics        |
| Grafana    | Dashboard      |
| Redis      | Caching        |
| NGINX      | Reverse proxy  |
| Docker     | Containers     |

---

# 23. MLOps Flow

```text
Training
   │
   ▼
Model Registry
   │
   ▼
Docker Build
   │
   ▼
CI/CD Pipeline
   │
   ▼
Kubernetes Deployment
   │
   ▼
Monitoring + Logging
```

---

# 24. CI/CD for Kubernetes

Typical workflow:

```text
GitHub Push
    │
    ▼
GitHub Actions
    │
    ▼
Docker Build
    │
    ▼
Push to DockerHub
    │
    ▼
Deploy to Kubernetes
```

Useful platforms:

* [GitHub Actions Documentation](https://docs.github.com/actions?utm_source=chatgpt.com)
* [FastAPI Documentation](https://fastapi.tiangolo.com/?utm_source=chatgpt.com)
* [Docker Documentation](https://docs.docker.com/?utm_source=chatgpt.com)

---

# 25. Why Kubernetes for ML?

| Feature             | Benefit                |
| ------------------- | ---------------------- |
| Auto Scaling        | Handles traffic spikes |
| Self Healing        | Restarts failed pods   |
| Load Balancing      | Distributes requests   |
| High Availability   | Multiple replicas      |
| Easy Rollback       | Safe deployments       |
| Resource Management | CPU/RAM control        |

---

# 26. Typical Interview Questions

### Why use Kubernetes for ML deployment?

* Scalability
* Fault tolerance
* Container orchestration
* Easy rolling updates

---

### Why FastAPI instead of Flask?

* Async support
* Faster performance
* Automatic Swagger docs
* Better validation using Pydantic

---

### Difference between Pod and Deployment?

| Pod                     | Deployment            |
| ----------------------- | --------------------- |
| Single running instance | Manages multiple pods |
| Temporary               | Persistent management |

---

# 27. Complete Workflow Summary

```text
Train ML Model
      │
      ▼
Save model.pkl
      │
      ▼
Create FastAPI API
      │
      ▼
Dockerize Application
      │
      ▼
Push Docker Image
      │
      ▼
Deploy on Kubernetes
      │
      ▼
Scale + Monitor
```

---

# 28. Next Advanced Topics

After this learn:

1. GPU deployment
2. Helm charts
3. Ingress controller
4. MLflow integration
5. Model versioning
6. Canary deployment
7. Blue-Green deployment
8. Kubeflow
9. KServe
10. Monitoring + Drift detection

---

# 29. Recommended Learning Order

```text
Docker
   ↓
FastAPI
   ↓
Kubernetes Basics
   ↓
Deploy ML APIs
   ↓
Scaling
   ↓
CI/CD
   ↓
Monitoring
   ↓
Advanced MLOps
```
