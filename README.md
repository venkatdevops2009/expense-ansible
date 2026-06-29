# 🚀 Expense Application Deployment using Ansible on AWS

![Ansible](https://img.shields.io/badge/Ansible-Automation-EE0000?logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazonaws&logoColor=white)
![RHEL9](https://img.shields.io/badge/RHEL-9-red?logo=redhat)
![MySQL](https://img.shields.io/badge/MySQL-Database-4479A1?logo=mysql&logoColor=white)
![NodeJS](https://img.shields.io/badge/Node.js-20-339933?logo=node.js&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-1.24-009639?logo=nginx&logoColor=white)

---

# 📖 Project Overview

This project automates the deployment of the **Expense Management Application** using **Ansible** on **AWS EC2 (RHEL 9)**.

The deployment consists of three independent servers:

- **Frontend Server** – Nginx
- **Backend Server** – Node.js Application
- **Database Server** – MySQL 8

Ansible automates the complete provisioning and configuration process, eliminating manual server setup.

---

# 🏗️ Architecture

```
                    Internet
                        │
                        ▼
                ┌────────────────┐
                │   Frontend     │
                │     Nginx      │
                └───────┬────────┘
                        │
                 Reverse Proxy
                        │
                        ▼
                ┌────────────────┐
                │    Backend     │
                │    Node.js     │
                └───────┬────────┘
                        │
                  MySQL Client
                        │
                        ▼
                ┌────────────────┐
                │     MySQL      │
                │   Database     │
                └────────────────┘
```

---

# 📂 Project Structure

```
expense-ansible/
│
├── group_vars/
   ├── all.yaml
├── inventory.ini
├── 01-mysql.yaml
├── 02-backend.yaml
├── 03-frontend.yaml
├── expense.yaml
├── backend.service.j2
├── expense.conf.j2
└── README.md
```

---

# ⚙️ Prerequisites

- AWS Account
- Three EC2 Instances (RHEL 9)
- Python 3
- Ansible
- SSH Connectivity
- Security Groups configured

---

# 🔐 Security Group Configuration

## Frontend

| Port | Purpose |
|------|----------|
|22|SSH|
|80|HTTP|

---

## Backend

| Port | Purpose |
|------|----------|
|22|SSH|
|8080|Application|

---

## MySQL

| Port | Purpose |
|------|----------|
|22|SSH|
|3306|MySQL|

---

# 📋 Inventory File

```ini
[local]
localhost

[mysql]
mysql.piridishop.shop

[backend]
backend.piridishop.shop

[frontend]
frontend.piridishop.shop
```

---

# 🚀 Deployment Steps

## Step 1

Create AWS servers using ansible sever first configure aws access key and secerate key in ansible server and also install boto3 and botocore for creating servers.

```bash
ansible-playbook -i inventory.ini expense.yaml
```

## Step 2
Configure MySQL

```bash
ansible-playbook -i inventory.ini 01-mysql.yaml
```

---

## Step 3

Configure Backend

```bash
ansible-playbook -i inventory.ini 02-backend.yaml
```

---

## Step 4

Configure Frontend

```bash
ansible-playbook -i inventory.ini 03-frontend.yaml
```

---

# 🗄️ MySQL Configuration

The MySQL playbook performs:

- Install MySQL Server
- Start MySQL Service
- Configure Root Password
- Create Expense User
- Grant Database Privileges

---

# ⚙️ Backend Configuration

The backend playbook performs:

- Enable Node.js 20 Module
- Install Node.js
- Create Expense System User
- Download Backend Artifact
- Install NPM Dependencies
- Import Database Schema
- Configure Systemd Service
- Start Backend Service

Environment Variables

```
DB_HOST
DB_USER
DB_PWD
DB_DATABASE
```

---

# 🌐 Frontend Configuration

The frontend playbook performs:

- Enable Nginx Module
- Install Nginx
- Download Frontend Artifact
- Configure Reverse Proxy
- Enable Nginx Service

Nginx forwards

```
/api
```

to

```
Backend:8080
```

---

# 🧪 Validation

## Backend Health

```bash
curl http://BACKEND_PRIVATE_IP:8080/health
```

Expected

```json
{
  "status":"ok"
}
```

---

## Frontend

Open

```
http://Frontend-Public-IP
```

---

# 📊 Services

## MySQL

```bash
systemctl status mysqld
```

---

## Backend

```bash
systemctl status backend
```

---

## Frontend

```bash
systemctl status nginx
```

---

# 🔍 Troubleshooting

## Backend

```bash
journalctl -u backend -f
```

---

## Nginx

```bash
nginx -t
```

---

## MySQL

```bash
mysql -u root -p
```

---

# 📌 Common Issues

### Backend returns 500

- Verify DB_HOST
- Verify MySQL credentials
- Verify schema import
- Verify database permissions

---

### Nginx won't start

```bash
nginx -t
```

Check

```
/etc/nginx/conf.d/
```

---

### Database Connection Failed

Verify

```
DB_HOST
DB_USER
DB_PWD
```

---

# 🛠 Technologies Used

- AWS EC2
- Ansible
- Nginx
- Node.js
- MySQL
- Linux (RHEL 9)
- Systemd

---

# 📈 Future Enhancements

- Convert playbooks into reusable Ansible Roles
- Use Ansible Galaxy role structure
- Encrypt secrets with Ansible Vault
- Dynamic Inventory using AWS EC2 Plugin
- CI/CD integration with Jenkins or GitHub Actions
- Idempotency improvements
- Automated testing with Molecule

---
