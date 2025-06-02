# 📌 Pinterest Clone AWS EC2 Deployment Guide (Docker-Based)

This README contains step-by-step instructions and issue fixes from scratch to deploy the Pinterest Clone project to an AWS EC2 instance using Docker and Docker Compose.

---

## 🚀 Project Stack

* Frontend: React + Vite
* Backend: Node.js + Express
* Docker + Docker Compose
* Deployment: AWS EC2 (Free Tier)

* 
![Screenshot (10)](https://github.com/user-attachments/assets/2272c82d-c710-4126-8921-0c1ce71a029c)
![Screenshot (11)](https://github.com/user-attachments/assets/1aea1e48-168e-4f65-b232-6fe262a44f02)
![Screenshot (12)](https://github.com/user-attachments/assets/dffa16dc-5a54-4cd1-bec9-b83c669d16a1)

---

## 🧰 Prerequisites

* AWS account (Free tier)
* GitHub repo: [Pinterest\_Clone](https://github.com/umang045/Pinterest_Clone)
* Basic knowledge of terminal and SSH

---

## ✅ EC2 Setup & Configuration

### 1. **Create EC2 Instance**

* Use Ubuntu (e.g., Ubuntu 20.04 LTS)
* Generate and download a new Key Pair (PEM file)
* Allocate and associate an Elastic IP

### 2. **Connect to EC2 via SSH**

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ubuntu@your-ec2-ip
```

---

## 📁 Prepare Environment on EC2

### 3. **Update and Install Docker**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

### 4. **Install Docker Compose**

```bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 5. **Clone the GitHub Repo**

```bash
git clone https://github.com/umang045/Pinterest_Clone.git
cd Pinterest_Clone
```

---

## 🔧 Prepare Project Files

### 6. **Environment Files**

**Backend: `backend/.env`**

```
PORT=3000
MONGO_URI=your_mongo_uri
```

**Frontend: `client/.env`**

```
VITE_API_ENDPOINT=http://localhost:3000
```

### 7. **Docker Compose File** (`docker-compose.yml`)

```yaml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "3000:3000"
    env_file:
      - ./backend/.env
    restart: always

  client:
    build: ./client
    ports:
      - "8080:80"
    env_file:
      - ./client/.env
    depends_on:
      - backend
    restart: always
```

---

## 🛠️ Common Errors and Fixes

### 🔴 Docker Permission Denied

**Error:** `connect: permission denied /var/run/docker.sock`

**Fix:**

```bash
sudo usermod -aG docker $USER
newgrp docker
```

### 🔴 React 404 with Vite + Nginx

**Fix:** Update `vite.config.js`:

```js
base: '/pintrest/',
```

Also ensure your Nginx Dockerfile has correct copy:

```Dockerfile
COPY --from=builder /app/dist /usr/share/nginx/html
```

---

## 🚀 Run the Project

```bash
docker compose up --build -d
```

### Access the site:

```
http://your-ec2-ip:8080
```

---

## 🔁 (Optional) Setup CI/CD with GitHub Actions

* SSH into EC2 with GitHub Actions
* Use secrets to store EC2 IP, user, and SSH private key
* Create `.github/workflows/deploy.yml` for auto-deployment

---

## ✅ Final Notes

* Always expose needed ports in your **EC2 Security Group** (e.g., 3000, 8080)
* Use **Elastic IP** to avoid dynamic IP changes
* Use `docker compose logs -f` to monitor live logs

---

**Maintained by:** Umang

📁 *Next time you can reuse this guide to speed up your deployment process!*
