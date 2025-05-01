
# 🚀 Node.js App Deployment on GKE using DockerHub
---

## 🌍 Real-World Scenario

You’re a DevOps Engineer at a startup building a microservice architecture. Your task is to deploy a Node.js-based microservice to GKE using a container image hosted on DockerHub. This setup is often used in CI/CD pipelines for development and testing environments.

---

## 📘 Project Overview

This project demonstrates how to containerize a simple Node.js application, push the image to DockerHub, and deploy it to a Google Kubernetes Engine (GKE) cluster.

---

## 🧱 Project Structure

```
nodejs-gke-deployment/
├── app/
│   ├── app.js
│   └── package.json
├── Dockerfile
├── deployment.yml
├── .dockerignore
├── .gitignore
```
---

## setup-file.py

```python
import os

# Define base path
base_path = "/home/lilia/VIDEOS/nodejs-gke-deployment"

# File structure and their contents
files = {
    "app/app.js": """const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Node.js App running on GKE 🚀'));
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
""",
    "app/package.json": """{
  "name": "gke-node-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
""",
    "Dockerfile": """FROM node:18
WORKDIR /app
COPY app/ .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
""",
    "deployment.yml": """apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: laly9999/gke-node-app:1
        ports:
        - containerPort: 3000

---
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: LoadBalancer
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
""",
    ".dockerignore": """node_modules
npm-debug.log
""",
    ".gitignore": """node_modules/
.env
"""
}

# Create directories and write files
for relative_path, content in files.items():
    full_path = os.path.join(base_path, relative_path)
    os.makedirs(os.path.dirname(full_path), exist_ok=True)
    with open(full_path, 'w') as f:
        f.write(content)

"✅ All files created successfully in /home/lilia/VIDEOS/nodejs-gke-deployment"


```


---

## ✅ Step-by-Step Instructions

### 1️⃣ Create Node.js Application

**File:** `app/app.js`
```js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => res.send('Node.js App running on GKE 🚀'));
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

**File:** `app/package.json`
```json
{
  "name": "gke-node-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

Install dependencies:
```bash
cd app
npm install
```

---

### 2️⃣ Create a DockerHub Image Repository

- Go to [DockerHub](https://hub.docker.com)
- Create a new public repository named: `gke-node-app`

---

### 3️⃣ Containerize the Application

**Dockerfile**
```Dockerfile
FROM node:18
WORKDIR /app
COPY app/ .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```

Build Docker image:
```bash
docker build -t laly9999/gke-node-app:1 .
```

---

### 4️⃣ Authenticate to DockerHub

```bash
docker login -u laly9999
```
Input your DockerHub access token when prompted.

---

### 5️⃣ Push Image to DockerHub

```bash
docker push laly9999/gke-node-app:1
```

---

### 6️⃣ Create Kubernetes Manifest File

**deployment.yml**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: laly9999/gke-node-app:1
        ports:
        - containerPort: 3000

---
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: LoadBalancer
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
```

---

### 7️⃣ Spin Up GKE Cluster

```bash
gcloud auth login
gcloud config set project <YOUR_PROJECT_ID>
gcloud container clusters create gke-node-cluster --num-nodes=2 --zone=us-central1-a
gcloud container clusters get-credentials gke-node-cluster --zone us-central1-a
```

---

### 8️⃣ Deploy Application to GKE

```bash
kubectl apply -f deployment.yml
```

---

### 9️⃣ Test the Application

```bash
kubectl get svc
```

Access the app in your browser using the `EXTERNAL-IP`:

```
http://<EXTERNAL-IP>
```

---

### 🔟 Clean Up Resources

```bash
kubectl delete -f deployment.yml
gcloud container clusters delete gke-node-cluster --zone us-central1-a
```

---

## 🧹 Recap of What You Learned

| Step | Description |
|------|-------------|
| Node.js App | Built a simple Express.js app |
| Docker | Containerized the app |
| DockerHub | Stored the image remotely |
| GKE | Deployed app to Kubernetes |
| kubectl | Managed Kubernetes objects |
| Cleanup | Removed resources to avoid billing |

---

## 🙋‍♀️ Author

**Liliane Konissi**  
GitHub: [@lily4499](https://github.com/lily4499)

---

## 📜 License

This project is open-source and available under the [MIT License](LICENSE).


---

