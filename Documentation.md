# 🚀 EasyCRUD – Full Stack Application Deployment on AWS

EasyCRUD is a **full-stack CRUD (Create, Read, Update, Delete) web application** deployed using **AWS services and Docker containers**.

The application architecture includes:

* **Frontend:** React + Apache/Nginx
* **Backend:** Spring Boot
* **Database:** MariaDB (Amazon RDS)
* **Compute:** Amazon EC2
* **Containerization:** Docker

This project demonstrates how to deploy a **containerized full-stack application using AWS infrastructure**.

---

# 🏗️ Architecture Overview

```
User Browser
     │
     ▼
Frontend (React + Apache) → Docker Container
     │
     ▼
Backend (Spring Boot API) → Docker Container
     │
     ▼
Amazon RDS (MariaDB)
```

---

# ⚙️ Prerequisites

Before starting, ensure you have:

* AWS Account
* EC2 Instance (Ubuntu)
* Docker installed
* RDS MariaDB instance
* Git installed
* Open required security group ports

---

# 📌 Step 1: Create Amazon RDS Database

1. Open **AWS Console**
2. Navigate to **RDS → Create Database**

### Configure the database

| Setting         | Value       |
| --------------- | ----------- |
| Engine          | MariaDB     |
| Template        | Free Tier   |
| Master Username | `admin`     |
| Master Password | `Redhat123` |

### Connectivity Settings

| Option         | Value               |
| -------------- | ------------------- |
| VPC            | Same VPC as EC2     |
| Public Access  | Yes                 |
| Security Group | Allow port **3306** |

Create database.

After creation, copy the **RDS Endpoint**.

---

# 📌 Step 2: Launch EC2 Instance

Launch an **Ubuntu EC2 instance**.

Connect using SSH:

```bash
ssh ubuntu@<EC2_PUBLIC_IP>
```

Update the system:

```bash
sudo apt update -y
```

---

# 📌 Step 3: Install MySQL Client & Connect RDS

Install MySQL client on EC2.

```bash
sudo apt install mysql-client -y
```

Connect to the RDS database using the endpoint.

```bash
mysql -h <RDS_ENDPOINT> -u admin -p
```

Enter the password you configured.

---

# 📌 Step 4: Create Database and Table

Run the following SQL commands.

### Create Database

```sql
SHOW DATABASES;
CREATE DATABASE student_db;
USE student_db;
```

### Create Table

```sql
CREATE TABLE students (
  id BIGINT NOT NULL AUTO_INCREMENT,
  name VARCHAR(255),
  email VARCHAR(255),
  course VARCHAR(255),
  student_class VARCHAR(255),
  percentage DOUBLE,
  branch VARCHAR(255),
  mobile_number VARCHAR(255),
  PRIMARY KEY (id)
);
```

---

# 📌 Step 5: Install Docker on EC2

Install Docker:

```bash
sudo apt install docker.io -y
```

Start Docker service:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Verify installation:

```bash
docker --version
```

---

# 📌 Step 6: Clone Project Repository

Clone the EasyCRUD repository.

```bash
git clone https://github.com/Rohit-1920/EasyCRUD.git
```

Navigate to the backend directory.

```bash
cd EasyCRUD/backend
```

---

# 📌 Step 7: Configure Backend Application

Copy the `application.properties` file to the root directory.

```bash
cp src/main/resources/application.properties .
```

Edit the configuration file:

```bash
nano application.properties
```

Update the following values:

```properties
server.port=8080

spring.datasource.url=jdbc:mariadb://<RDS_ENDPOINT>:3306/student_db
spring.datasource.username=admin
spring.datasource.password=Redhat123

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

Save the file.

---

# 📌 Step 8: Create Backend Dockerfile

Create a Dockerfile.

```bash
nano Dockerfile
```

Add the following content.

```dockerfile
FROM maven:3.8.5-openjdk-17

COPY . /opt/
WORKDIR /opt

RUN rm -rf src/main/resources/application.properties
RUN cp application.properties src/main/resources/

RUN mvn clean package

WORKDIR target/

CMD ["java","-jar","student-registration-backend-0.0.1-SNAPSHOT.jar"]
```

---

# 📌 Step 9: Build Backend Docker Image

Build the backend Docker image.

```bash
docker build -t backend:v1 .
```

Verify the image.

```bash
docker images
```

---

# 📌 Step 10: Run Backend Docker Container

Run the backend container.

```bash
docker run -d -p 8080:8080 backend:v1
```

Verify container status.

```bash
docker ps
```

---

# 📌 Step 11: Verify Backend Deployment

Open the following URL in your browser:

```
http://<EC2_PUBLIC_IP>:8080
```

If the API responds successfully, the backend is running.

---

# 📌 Step 12: Configure Frontend Application

Navigate to the frontend directory.

```bash
cd ../frontend
```

List files including hidden files.

```bash
ls -a
```

Edit the `.env` file.

```bash
nano .env
```

Update the backend API URL.

```env
VITE_API_URL=http://<EC2_PUBLIC_IP>:8080/api
```

Save the file.

---

# 📌 Step 13: Create Frontend Dockerfile

Create the Dockerfile.

```bash
nano Dockerfile
```

Add the following content.

```dockerfile
FROM node:18-alpine

COPY . /opt/
WORKDIR /opt

RUN npm install
RUN npm run build

RUN apk update && apk add apache2

RUN cp -rf dist/* /var/www/localhost/htdocs/

EXPOSE 80

CMD ["httpd","-D","FOREGROUND"]
```

---

# 📌 Step 14: Build Frontend Docker Image

Build the image.

```bash
docker build -t frontend:v1 .
```

Verify the image.

```bash
docker images
```

---

# 📌 Step 15: Run Frontend Container

Run the frontend container.

```bash
docker run -d -p 80:80 frontend:v1
```

Verify the container.

```bash
docker ps
```

---

# 📌 Step 16: Verify Frontend Deployment

Open your browser and navigate to:

```
http://<EC2_PUBLIC_IP>
```

You should see the **EasyCRUD application UI**.

---

# 🎉 Application Successfully Deployed

Your full-stack application is now live.

```
Frontend (React)
      │
      ▼
Backend (Spring Boot API)
      │
      ▼
Amazon RDS (MariaDB)
```

---

# 📊 Technologies Used

| Technology   | Purpose             |
| ------------ | ------------------- |
| AWS EC2      | Application hosting |
| Amazon RDS   | Managed database    |
| Docker       | Containerization    |
| Spring Boot  | Backend API         |
| React        | Frontend UI         |
| Apache/Nginx | Web server          |

---
These will make your **DevOps portfolio much stronger.**
