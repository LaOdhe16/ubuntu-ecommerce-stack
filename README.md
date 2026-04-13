# 🛒 Project: Ubuntu Server E-Commerce & Management Dashboard

## 📝 Project Overview
This project demonstrates the deployment of a robust, self-hosted **Web Server** infrastructure using **Ubuntu Server 22.04 LTS**. The system integrates a web-based administration interface via **Cockpit Dashboard** with a fully functional e-commerce platform powered by **WordPress and WooCommerce**.

---

## 🖥️ Server Specifications & Environment
The infrastructure is built upon **Ubuntu Server**, a headless Linux distribution optimized for high-performance server workloads.

### **Hardware Specifications (Virtual Environment):**
Based on the current virtual machine configuration:
* **Operating System:** Ubuntu (64-bit)
* **Base Memory (RAM):** 7247 MB (~7 GB)
* **Processors:** 5 CPU Cores
* **Storage:** 25.00 GB VDI (SATA)
* **Graphics Controller:** VMSVGA (16 MB Video Memory)
* **Acceleration:** Nested Paging, KVM Paravirtualization

### **Software Stack:**
* **Web Server:** Apache2
* **Database:** MySQL Server
* **Language:** PHP (with required WordPress extensions)
* **Management UI:** Cockpit Dashboard (Port 9090)

### **Why Ubuntu Server?**
* **Efficiency:** Headless operation ensures maximum RAM and CPU availability for the web application.
* **Stability:** Optimized for 24/7 uptime and long-term support.
* **Security:** Features a hardened kernel and granular user permission management.

---

## 🛠️ Network Configuration (Port Forwarding)
Since the server operates within a NAT environment, the following **Port Forwarding** rules must be configured in VirtualBox to allow the Host browser to communicate with the Guest server:

| Service Name | Protocol | Host IP | Host Port | Guest Port |
| :--- | :--- | :--- | :--- | :--- |
| **Cockpit (Dashboard)** | TCP | 127.0.0.1 | 9090 | 9090 |
| **HTTP (E-commerce)** | TCP | 127.0.0.1 | 80 | 80 |

---

## 🚀 Installation & Setup Guide

### 1. System Dashboard (Cockpit)
Cockpit provides a real-time visual overview of system health.
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install cockpit -y
sudo systemctl enable --now cockpit.socket
```
Access via: https://localhost:9090

<img width="1919" height="972" alt="Screenshot 2026-04-13 091920" src="https://github.com/user-attachments/assets/61b7739c-6e2a-4a2b-84ec-0d8a1d42f31c" />

### 2. Web Stack (LAMP) & PHP Dependencies
Installing the engine required to serve the e-commerce platform.
```bash
sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php php-xml php-gd php-mbstring php-curl php-zip php-intl php-soap -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

### 3. Database Configuration
Creating a secure database for product data and user transactions.
```bash
sudo mysql
```
Run the following SQL commands:
```bash
CREATE DATABASE toko_online;
CREATE USER 'admin_toko'@'localhost' IDENTIFIED BY 'AdminPaling.Top17';
GRANT ALL PRIVILEGES ON toko_online.* TO 'admin_toko'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 4. WordPress & WooCommerce Deployment
Downloading the core CMS and setting up directory permissions.
```bash
cd /var/www/html
sudo rm index.html
sudo wget [https://wordpress.org/latest.tar.gz](https://wordpress.org/latest.tar.gz)
sudo tar -xvf latest.tar.gz
sudo mv wordpress/* .
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
sudo rm -rf wordpress latest.tar.gz
```

### 5. Finalizing the E-commerce Store
* Open your browser and navigate to http://localhost.
* Link the database using the credentials: DB: toko_online, User: admin_toko, Pass: AdminPaling.Top17.
* Once in the WordPress Dashboard, navigate to Plugins > Add New.
* Search for "WooCommerce", then Install and Activate.
* Complete the setup wizard to configure currency (IDR/USD), payments, and shipping.

<img width="1919" height="970" alt="Screenshot 2026-04-13 092059" src="https://github.com/user-attachments/assets/b29d3137-65e0-46f4-9138-3becc2cf665e" />


## 📉 System Monitoring
* Using the Cockpit Dashboard, you can monitor:
* CPU & RAM Usage: Track spikes during high-traffic shopping sessions.
* Service Management: Ensure apache2 and mysql services are always running.
* System Logs: Debug errors or unauthorized access attempts in real-time.
