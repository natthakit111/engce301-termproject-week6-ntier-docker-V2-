## engce301 - Software Architecture

**Week 6: Containerization with Docker**

---

# 📌 Project Overview

โปรเจกต์นี้เป็นระบบ **Task Board Management System** ที่ถูกออกแบบตามแนวคิด **N-Tier Architecture** และทำการ **Containerize ด้วย Docker**

ระบบประกอบด้วย 3 ชั้นหลัก:

* **Frontend Layer** → Static Web Interface
* **Application Layer** → Node.js REST API
* **Data Layer** → PostgreSQL Database

โดยใช้ **Nginx เป็น Reverse Proxy** เพื่อจัดการ HTTP/HTTPS และ routing request ไปยัง API

---

# 🏗️ System Architecture

```
Browser
   │
   ▼
Nginx (Reverse Proxy + SSL)
   │
   ▼
Node.js API (Express)
   │
   ▼
PostgreSQL Database
```

Architecture นี้เรียกว่า **3-Tier / N-Tier Architecture**

| Layer              | Technology        |
| ------------------ | ----------------- |
| Web Server         | Nginx             |
| Backend API        | Node.js + Express |
| Database           | PostgreSQL        |
| Container Platform | Docker            |
| Orchestration      | Docker Compose    |

---

# 📂 Project Structure

```
week6-ntier-docker/
│
├── docker-compose.yml
├── .env
├── .env.example
│
├── api/
│   ├── Dockerfile
│   ├── server.js
│   └── src/
│
├── nginx/
│   ├── nginx.conf
│   ├── conf.d/
│   └── ssl/
│
├── frontend/
│   ├── index.html
│   ├── css/
│   └── js/
│
├── database/
│   └── init.sql
│
├── scripts/
│   ├── start.sh
│   ├── stop.sh
│   ├── logs.sh
│   ├── test-api.sh
│   └── generate-ssl.sh
│
└── docs/
    └── ANALYSIS.md
```

---

# ⚙️ Technologies Used

* **Docker**
* **Docker Compose**
* **Node.js**
* **Express.js**
* **PostgreSQL**
* **Nginx**
* **HTML / CSS / JavaScript**

---

# 🚀 How to Run the Project

## 1️⃣ Clone Repository

```
git clone https://github.com/YOUR_USERNAME/week6-ntier-docker.git
cd week6-ntier-docker
```

---

## 2️⃣ Generate SSL Certificate

```
./scripts/generate-ssl.sh
```

---

## 3️⃣ Start All Containers

```
docker compose up -d
```

Docker Compose จะสร้าง container ดังนี้:

| Service | Description                   |
| ------- | ----------------------------- |
| db      | PostgreSQL Database           |
| api     | Node.js Backend API           |
| nginx   | Reverse Proxy + Static Server |

---

## 4️⃣ ตรวจสอบสถานะ Container

```
docker compose ps
```

---

## 5️⃣ เข้าใช้งานระบบ

Frontend

```
http://localhost
```

API Health Check

```
http://localhost/api/health
```

---

# 🔧 Useful Docker Commands

Start containers

```
docker compose up -d
```

Stop containers

```
docker compose down
```

View logs

```
docker compose logs
```

Restart service

```
docker compose restart api
```

Check resource usage

```
docker stats
```

---

# 🧪 API Endpoints

| Method | Endpoint         | Description     |
| ------ | ---------------- | --------------- |
| GET    | /api/health      | Health check    |
| GET    | /api/tasks       | Get all tasks   |
| GET    | /api/tasks/:id   | Get task by ID  |
| POST   | /api/tasks       | Create new task |
| PUT    | /api/tasks/:id   | Update task     |
| DELETE | /api/tasks/:id   | Delete task     |
| GET    | /api/tasks/stats | Task statistics |

---

# 🗄️ Database

Database: **PostgreSQL**

Default configuration

```
DB_NAME=taskboard_db
DB_USER=taskboard
DB_PASSWORD=taskboard123
```

Database schema ถูกสร้างอัตโนมัติผ่านไฟล์

```
database/init.sql
```

---

# 🔐 SSL Configuration

SSL certificate ถูกสร้างผ่าน script

```
scripts/generate-ssl.sh
```

และถูก mount ไปยัง nginx container

```
nginx/ssl/
```

---

# 📊 VM vs Docker Analysis

การวิเคราะห์เปรียบเทียบระหว่าง

* **Virtual Machine Deployment**
* **Docker Deployment**

อยู่ในไฟล์

```
docs/ANALYSIS.md
```

---

# 🛠️ Troubleshooting

### Containers ไม่ start

```
docker compose logs
```

Restart ใหม่

```
docker compose down
docker compose up -d
```

---

### Port already in use

ตรวจสอบ port

```
sudo lsof -i :80
sudo lsof -i :443
```

หยุด nginx บน host

```
sudo systemctl stop nginx
```

---

### Database connect ไม่ได้

ดู logs

```
docker compose logs db
```

เข้า database

```
docker exec -it taskboard-db psql -U taskboard -d taskboard_db
```

---

### Clean restart

```
docker compose down -v
docker system prune -f
docker compose up -d --build
```

---

# 📚 Learning Outcomes

จาก Lab นี้ได้เรียนรู้

* การสร้าง **N-Tier Architecture**
* การใช้ **Docker Containerization**
* การใช้ **Docker Compose สำหรับ Multi-Container Application**
* การตั้งค่า **Reverse Proxy ด้วย Nginx**
* การใช้งาน **Docker Network และ Volumes**

---

# 👨‍🎓 Student Information

**Name:** ณัฐกิตติ์ ยั่งยืนปิยรัตน์
 **Course:** ENGCE Software Architecture
 **University:** Rajamangala University of Technology Lanna

---

