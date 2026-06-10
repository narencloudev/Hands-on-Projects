# 🚀 DevOps Capstone Project (AWS Free Tier)

## 📌 Project Overview

This project demonstrates a complete DevOps CI/CD pipeline built using AWS Free Tier services and modern DevOps tools.

The application is containerized using Docker, automatically built and scanned through GitHub Actions, stored in Amazon ECR, and prepared for Kubernetes deployment using GitOps practices.

## 🏗️ Architecture

```text
Developer
   ↓
GitHub
   ↓
GitHub Actions
   ↓
Docker Build
   ↓
Trivy Security Scan
   ↓
Amazon ECR
   ↓
ArgoCD
   ↓
K3s Kubernetes
   ↓
Prometheus
   ↓
Grafana
```

---

## 🛠️ Tools & Technologies Used

| Tool           | Purpose                    |
| -------------- | -------------------------- |
| GitHub         | Source Code Repository     |
| GitHub Actions | CI/CD Automation           |
| Docker         | Containerization           |
| Trivy          | Security Scanning          |
| Amazon ECR     | Container Registry         |
| AWS IAM        | Secure Access Management   |
| AWS EC2        | Hosting & Infrastructure   |
| K3s            | Lightweight Kubernetes     |
| ArgoCD         | GitOps Deployment          |
| Prometheus     | Monitoring                 |
| Grafana        | Visualization & Dashboards |

---

## 📂 Project Structure

```text
devops-project
│
├── .github
│   └── workflows
│       └── deploy.yml
│
├── Dockerfile
├── app.js
├── package.json
├── package-lock.json
│
├── k8s
│   ├── deployment.yaml
│   └── service.yaml
│
└── README.md
```

---

## ⚙️ Application

### app.js

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('DevOps Capstone Running Successfully 🚀');
});

app.listen(port, () => {
  console.log(`App running at http://localhost:${port}`);
});
```

---

## 🐳 Docker Containerization

### Build Image

```bash
docker build -t devops-project .
```

### Run Container

```bash
docker run -d -p 3000:3000 --name devops-container devops-project
```

### Verify

```bash
curl http://localhost:3000
```

Output:

```text
DevOps Capstone Running Successfully 🚀
```

---

## ☁️ Amazon ECR Setup

### Create Repository

```bash
aws ecr create-repository \
--repository-name devops-project \
--region ap-south-1
```

### Login to ECR

```bash
aws ecr get-login-password --region ap-south-1 | \
docker login --username AWS --password-stdin \
ACCOUNT_ID.dkr.ecr.ap-south-1.amazonaws.com
```

---

## 🔄 CI/CD Pipeline

### Workflow

1. Developer pushes code to GitHub
2. GitHub Actions workflow starts automatically
3. Docker image is built
4. Trivy performs security scanning
5. Image is tagged
6. Image is pushed to Amazon ECR
7. ArgoCD detects deployment changes
8. Kubernetes updates application

---

## 🔐 Security Scanning

Trivy scans container images for:

* Critical vulnerabilities
* High severity vulnerabilities
* Misconfigurations
* Exposed secrets

Pipeline can be configured to fail if critical vulnerabilities are detected.

---

## ☸️ Kubernetes Deployment

### Deployment

```yaml
replicas: 2
```

Features:

* Multiple replicas
* Self-healing
* Rolling updates
* High availability

### Service

```yaml
type: LoadBalancer
```

Exposes the application externally.

---

## 📊 Monitoring & Observability

### Prometheus

Collects:

* CPU usage
* Memory usage
* Pod metrics
* Node metrics

### Grafana

Provides:

* Real-time dashboards
* Infrastructure monitoring
* Application monitoring

---

## 🎯 Key DevOps Concepts Demonstrated

* Continuous Integration (CI)
* Continuous Delivery (CD)
* Containerization
* Infrastructure Automation
* Security Scanning
* Kubernetes Orchestration
* GitOps
* Monitoring & Observability

---

## 📈 Current Status

### Completed

* [x] Node.js Application
* [x] Docker Containerization
* [x] GitHub Repository
* [x] AWS IAM Role
* [x] Amazon ECR
* [x] GitHub Actions CI Pipeline
* [x] Automated Docker Build
* [x] Automated ECR Push

### In Progress

* [ ] Trivy Security Scanning
* [ ] K3s Kubernetes Cluster
* [ ] ArgoCD GitOps
* [ ] Prometheus Monitoring
* [ ] Grafana Dashboards

---

## 👨‍💻 Author

**Naren Kanugu**

Cloud & DevOps Engineer

---

## ⭐ Project Goal

Build a production-style DevOps pipeline using AWS Free Tier services while implementing industry-standard CI/CD, containerization, GitOps, security, and monitoring practices.
