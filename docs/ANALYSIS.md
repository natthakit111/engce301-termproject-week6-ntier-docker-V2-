# 📊 การวิเคราะห์เปรียบเทียบ: VM vs Docker Deployment

## ENGCE301 - Week 6 N-Tier Architecture

**ชื่อ-นามสกุล:** ณัฐกิตติ์ ยั่งยืนปิยรัตน์
**รหัสนักศึกษา:** 66543206014-3

---

# 1. ตารางเปรียบเทียบ Setup Process

| ขั้นตอน            | Version 1 (VM)                                                              | Version 2 (Docker)                                                         |
| ------------------ | --------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| ติดตั้ง PostgreSQL | ต้องติดตั้งด้วย `apt install postgresql` และตั้งค่า user/password ด้วยตนเอง | ใช้ image `postgres:16-alpine` ใน docker-compose ทำให้ระบบติดตั้งอัตโนมัติ |
| ติดตั้ง Node.js    | ต้องติดตั้ง Node.js และ npm ด้วยคำสั่ง `apt install nodejs npm`             | ใช้ Dockerfile ที่ build จาก `node:20-alpine`                              |
| ติดตั้ง Nginx      | ต้องติดตั้งด้วย `apt install nginx` และแก้ config เอง                       | ใช้ image `nginx:alpine` และ mount config ผ่าน docker-compose              |
| Configure Database | ต้องสร้าง database และ table เองผ่าน PostgreSQL                             | ใช้ `init.sql` ให้ Docker สร้างอัตโนมัติ                                   |
| Configure SSL      | ต้อง generate SSL certificate และ config nginx เอง                          | ใช้ SSL certificate ที่เตรียมไว้ในโฟลเดอร์ nginx/ssl                       |
| Start Services     | ต้อง start service ทีละตัว เช่น `systemctl start nginx`                     | ใช้คำสั่งเดียว `docker compose up -d`                                      |
| **เวลาทั้งหมด**    | ประมาณ **30–40 นาที**                                                       | ประมาณ **5–10 นาที**                                                       |

---

# 2. ตารางเปรียบเทียบ Resource Usage

| Resource     | Version 1 (VM)                       | Version 2 (Docker)                     |
| ------------ | ------------------------------------ | -------------------------------------- |
| Memory Usage | ประมาณ 800MB – 1GB                   | ประมาณ 300MB – 500MB                   |
| Disk Usage   | ประมาณ 3GB – 5GB                     | ประมาณ 1GB – 2GB                       |
| CPU Usage    | ใช้ CPU มากกว่าเพราะมี OS เต็มรูปแบบ | ใช้ CPU น้อยกว่าเพราะใช้ shared kernel |
| Startup Time | ประมาณ 30–60 วินาที                  | ประมาณ 5–10 วินาที                     |

---

# 3. ข้อดีของ Docker Deployment

1. **ติดตั้งรวดเร็ว:**
   สามารถติดตั้งระบบทั้งหมดได้ด้วยคำสั่งเดียว `docker compose up -d`

2. **Environment เหมือนกันทุกเครื่อง:**
   Docker image ทำให้ environment เหมือนกันทุกเครื่อง ลดปัญหา “works on my machine”

3. **จัดการ service ได้ง่าย:**
   สามารถ start, stop หรือ restart service ทั้งหมดพร้อมกันได้

4. **ใช้ resource น้อยกว่า VM:**
   Docker ใช้ shared kernel ทำให้ใช้ memory และ CPU น้อยกว่า

5. **ง่ายต่อการ deploy:**
   สามารถ deploy ไปยัง server หรือ cloud ได้ง่าย เช่น AWS, Railway หรือ Kubernetes

---

# 4. ข้อเสียของ Docker Deployment

1. **ต้องเรียนรู้ Docker เพิ่ม:**
   ผู้ใช้ต้องเข้าใจ concepts เช่น container, image, volume และ network

2. **Debug อาจซับซ้อน:**
   เมื่อเกิดปัญหา ต้องตรวจสอบผ่าน `docker logs` หรือ `docker inspect`

3. **Security ต้องจัดการดี:**
   หากตั้งค่า container ไม่ถูกต้อง อาจเกิด security risk ได้

---

# 5. เมื่อไหร่ควรใช้ VM vs Docker?

### ควรใช้ VM เมื่อ:

* ต้องการจำลองระบบปฏิบัติการเต็มรูปแบบ
* ต้องการ isolation สูง เช่นระบบ production ที่ต้องการ security สูง
* ใช้ software ที่ต้องการ OS เฉพาะ

### ควรใช้ Docker เมื่อ:

* ต้องการ deploy application อย่างรวดเร็ว
* ใช้ microservices architecture
* ต้องการ environment ที่เหมือนกันทุกเครื่อง

---

# 6. สิ่งที่ได้เรียนรู้จาก Lab นี้

จาก Lab นี้ได้เรียนรู้เกี่ยวกับการสร้างระบบ N-Tier Architecture โดยใช้ Docker ซึ่งประกอบด้วย Nginx, Node.js API และ PostgreSQL database นอกจากนี้ยังได้เรียนรู้การใช้ Dockerfile และ docker-compose ในการจัดการ container หลายตัวพร้อมกัน รวมถึงการตั้งค่า reverse proxy ด้วย Nginx และการใช้งาน health check เพื่อให้ระบบมีความเสถียรมากขึ้น การใช้ Docker ยังช่วยให้การ deploy และการจัดการระบบทำได้ง่ายและรวดเร็วกว่า VM แบบเดิม

---

# 7. คำสั่ง Docker ที่ใช้บ่อย (Quick Reference)

```bash
# Start containers
docker compose up -d

# Stop containers
docker compose down

# ดูสถานะ container
docker compose ps

# ดู logs
docker compose logs

# ดู memory usage ของ Docker
docker stats --no-stream

# ดู disk usage
docker system df

# ดู container sizes
docker ps -s

# ดู image sizes
docker images

# ดู network
docker network ls

docker network inspect taskboard-network
```

สร้างโดย: ณัฐกิตติ์ ยั่งยืนปิยรัตน์
ENGCE301 Software Architecture - Week 6
