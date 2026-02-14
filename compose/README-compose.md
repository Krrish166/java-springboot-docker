
```markdown
# 🚀 Spring Boot + Streamlit Multi-Container Deployment (Docker Compose)

## 📌 Project Overview

This project demonstrates a production-style multi-container application using:

- 🔹 Spring Boot Backend (Java)
- 🔹 Streamlit Frontend (Python)
- 🔹 AWS RDS (MySQL)
- 🔹 Docker & Docker Compose
- 🔹 EC2 Deployment

The application allows student management operations (Add, View, Update, Delete) through a web UI.

---

# 🏗 Architecture

```

User → EC2 (Port 8501)
↓
Frontend (Streamlit)
↓
Backend (Spring Boot API)
↓
AWS RDS (MySQL)

````

Both services run inside Docker containers and communicate through a shared Docker bridge network.

---

# 🖥 EC2 Setup (Amazon Linux)

## 1️⃣ Install Docker

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl enable docker
sudo systemctl start docker
````

---

## 2️⃣ Allow ec2-user to Use Docker

```bash
sudo usermod -aG docker ec2-user
```

Logout and reconnect to apply changes.

Verify:

```bash
docker ps
```

---

## 3️⃣ Open Required Security Group Ports

| Port | Purpose                                    |
| ---- | ------------------------------------------ |
| 22   | SSH                                        |
| 8501 | Frontend (Streamlit UI)                    |
| 8084 | Backend API (Optional, internal preferred) |

---

# 📂 Project Structure

```

backend/         → Spring Boot application
frontend/        → Streamlit application
compose/
  ├── docker-compose-rds.yaml
  └── .env.rds

```

---

# 🔐 Environment Configuration

Inside `compose/.env.rds`:

```
SPRING_DATASOURCE_URL=jdbc:mysql://<RDS-ENDPOINT>:3306/<DATABASE_NAME>
SPRING_DATASOURCE_USERNAME=admin
SPRING_DATASOURCE_PASSWORD=yourpassword
API_URL=http://backend-app:8084
FRONTEND_PORT=8501
BACKEND_PORT=8084
```

---

# 🐳 Running with Docker Compose

Navigate to compose directory:

```bash
cd compose
```

Start services:

```bash
docker compose -f docker-compose-rds.yaml up --build -d
```

---

# 🔍 Verify Deployment

```bash
docker ps
```

Check logs:

```bash
docker logs backend-app
docker logs frontend-app
```

Access application:

```
http://<EC2_PUBLIC_IP>:8501
```

---

# 🛑 Stop Application

```bash
docker compose down
```

---

# 🔄 Rebuild After Code Changes

```bash
docker compose -f docker-compose-rds.yaml up --build -d
```

---

# 🌐 Networking

* Both containers are attached to a shared Docker bridge network.
* Frontend communicates with backend using container name:

```
http://backend-app:8084
```

* Backend connects securely to AWS RDS using environment variables.

---

# ✅ Deployment Outcome

✔ Fully containerized multi-service application
✔ Docker Compose orchestration
✔ EC2-hosted environment
✔ RDS-integrated database backend
✔ Internal container networking
✔ Production-ready structure

---

# 📦 Tech Stack

* Java 11 / Spring Boot
* Python 3.10 / Streamlit
* Docker
* Docker Compose
* AWS EC2
* AWS RDS (MySQL)

---

# 🎯 Summary

This project demonstrates:

* Multi-container Docker architecture
* Environment-based configuration
* Cloud database integration
* Production-style EC2 deployment
* Clean service-to-service communication

A complete end-to-end DevOps-ready containerized application.

```



Just tell me 👌
```
