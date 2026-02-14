
```markdown
# 🚀 CI/CD Deployment with GitHub Actions + AWS ECR + EC2

## 📌 Project Overview

This implementation provides a fully automated CI/CD pipeline for:

- 🔹 Spring Boot Backend (Dockerized)
- 🔹 Streamlit Frontend (Dockerized)
- 🔹 AWS RDS (MySQL)
- 🔹 Amazon ECR (Container Registry)
- 🔹 EC2 (Container Host)
- 🔹 GitHub Actions (Automation Engine)

Every push to the `docker` branch automatically:

1. Builds Docker images
2. Pushes images to Amazon ECR
3. Connects to EC2 via SSH
4. Pulls latest images
5. Deploys updated containers

---

 🏗 CI/CD Architecture

```

Developer Push → GitHub
↓
GitHub Actions
↓
Build Docker Images
↓
Push to Amazon ECR
↓
SSH into EC2
↓
Pull & Deploy Containers
↓
Frontend → Backend → RDS

```

---

# 📂 Pipeline Location

```

.github/workflows/ecr-cicd.yml

```

Trigger Branch:

```

docker

```

---

# 🔐 Required GitHub Secrets

Go to:

**GitHub → Settings → Secrets and Variables → Actions**

Add the following:

```

AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_REGION
AWS_ACCOUNT_ID
EC2_HOST
EC2_USER
EC2_KEY
SPRING_DATASOURCE_URL
SPRING_DATASOURCE_USERNAME
SPRING_DATASOURCE_PASSWORD

````

---

# 🖥 EC2 Pre-Configuration

Before running the pipeline, configure EC2.

## 1️⃣ Install Docker

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl enable docker
sudo systemctl start docker
````

---

## 2️⃣ Allow ec2-user to Run Docker

```bash
sudo usermod -aG docker ec2-user
```

Logout and reconnect.

Verify:

```bash
docker ps
```

---

## 3️⃣ Create Docker Network

```bash
docker network create appnet
```

---

## 4️⃣ Open Required Security Group Ports

| Port | Purpose                              |
| ---- | ------------------------------------ |
| 22   | SSH                                  |
| 8501 | Frontend UI                          |
| 8084 | Backend API (optional public access) |

---

# 🔄 How to Trigger CI/CD

Create and push to docker branch:

```bash
git checkout -b docker
git add .
git commit -m "Trigger CI/CD deployment"
git push origin docker
```

GitHub Actions will automatically start the deployment process.

---

# ⚙️ What the Pipeline Does

## On GitHub Runner:

* Configures AWS credentials
* Logs into Amazon ECR
* Builds backend Docker image
* Builds frontend Docker image
* Tags images
* Pushes images to ECR

---

## On EC2:

* Logs into ECR
* Stops and removes old containers
* Pulls latest images
* Runs backend container
* Runs frontend container
* Connects containers via Docker network

---

# 🐳 Container Configuration

## Backend

* Runs on port `8084`
* Uses environment variables for RDS configuration
* Connected to Docker network `appnet`

## Frontend

* Runs on port `8501`
* Uses internal API URL:

```
http://backend-app:8084
```

---

# 🔍 Verification Commands (On EC2)

```bash
docker ps
docker logs backend-app
docker logs frontend-app
```

Access application:

```
http://<EC2_PUBLIC_IP>:8501
```

---

# 🛑 Manual Container Restart (If Required)

```bash
docker stop backend-app frontend-app
docker rm backend-app frontend-app
```

---

# 🛡 Security Considerations

* Backend can remain internal-only
* Use Security Groups to restrict port access
* Do not hardcode secrets in Dockerfiles
* Use GitHub Secrets for sensitive data

---

# 📦 Tech Stack

* Spring Boot (Java)
* Streamlit (Python)
* Docker
* GitHub Actions
* AWS ECR
* AWS EC2
* AWS RDS (MySQL)

---

# ✅ Deployment Outcome

✔ Automated Docker image builds
✔ Image storage in Amazon ECR
✔ Automated EC2 deployment
✔ Zero manual intervention
✔ Secure RDS integration
✔ Production-ready CI/CD workflow

---

# 🎯 Summary

This project demonstrates:

* End-to-end CI/CD automation
* Containerized microservice architecture
* Cloud-native deployment
* Secure secret management
* DevOps production workflow implementation

A complete real-world DevOps pipeline from code commit to live deployment.

```

---

```
