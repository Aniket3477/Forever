his section explains how to deploy the full-stack e-commerce app using Kubernetes and Minikube.

### 📦 Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Docker (for building images)

---

### 🧱 Folder Structure
k8s/
├── backend/
│   ├── backend_deployment.yaml
│   ├── backend_service.yaml
│   ├── backend_configMap.yaml
├── mongo/
│   ├── mongo_deployment.yaml
│   ├── mongo_service.yaml
│   ├── mongo_secret.yaml
├── frontend/
│   ├── frontend_deployment.yaml
│   ├── frontend_service.yaml
---

### ⚙️ Step-by-Step Deployment

#### 1. Start Minikube

        minikube start

3. Push Docker Images to Docker Hub
Build and push your images:
# Backend
     docker build -t aniketsaini/backend:v3 ./backend
     docker push aniketsaini/backend:v3

# Frontend
     docker build -t aniketsaini777/frontend:v1 ./frontend
     docker push aniketsaini777/frontend:v1


Replace aniketsaini777 with your actual Docker Hub username.


4. Update your deployment YAMLs to use:
        image: aniketsaini/backend:v3
        image: aniketsaini/frontend:v1



5. Apply Kubernetes Manifests
kubectl apply -f k8s/mongo/mongo_secret.yaml
kubectl apply -f k8s/mongo/mongo_deployment.yaml
kubectl apply -f k8s/mongo/mongo_service.yaml

kubectl apply -f k8s/backend/backend_configMap.yaml
kubectl apply -f k8s/backend/backend_deployment.yaml
kubectl apply -f k8s/backend/backend_service.yaml

kubectl apply -f k8s/frontend/frontend_deployment.yaml
kubectl apply -f k8s/frontend/frontend_service.yaml



🔌 Accessing the App
Option 1: Port-Forward
kubectl port-forward svc/backend 4000:4000 -n e-commerce-web
curl http://localhost:4000/api/user


Option 2: Ingress
kubectl apply -f k8s/ingress.yaml
curl http://<minikube-ip>/api/user


Get Minikube IP:
minikube ip



🧪 Sample API Test
curl -X POST http://localhost:4000/api/user/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Aniket","email":"aniket@example.com","password":"securepass"}'




