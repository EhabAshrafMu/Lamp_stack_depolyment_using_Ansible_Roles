# LAMP Stack Deployment using Ansible

This project provides a fully automated deployment of a **LAMP stack** (Linux, Apache, MySQL, PHP) using **Ansible**. It follows an organized **Ansible role-based architecture**, making it modular and reusable for real-world infrastructure automation.

---

## 🧱 Project Structure

```bash
lamp-stack/
├── ansible.cfg                # Ansible configuration
├── inventory/
│   └── hosts                  # Inventory file defining db and web hosts
├── group_vars/
│   ├── dbservers.yaml         # Group variables for DB servers
│   └── webservers.yaml        # Group variables for Web servers
├── playbook.yaml              # Main playbook to orchestrate roles
├── roles/
│   ├── dbserver/              # Role to configure MySQL database server
│   └── webserver/             # Role to configure Apache + PHP web server
└── README.md                  # This file
```

---

## 💡 Overview

This setup includes:

- ✅ Apache2 web server (HTTPD)
- ✅ PHP support with apache2-mod-php equivalent on RHEL
- ✅ MySQL 8.x with root password configuration
- ✅ `phpinfo.php` for verification
- ✅ Use of `mysql.cnf` for secure CLI authentication

---

## 🔧 Requirements

- Ansible installed on the control node
- RHEL 8 or compatible servers for DB and Web
- SSH access to target nodes
- Python3 and `pip` installed

---

## 📦 Roles Summary

### `webserver` Role
- Installs Apache and PHP
- Configures Apache using a Jinja2 template
- Deploys a `phpinfo.php` file to test PHP processing

### `dbserver` Role
- Installs MySQL server and Python connectors
- Starts and enables MySQL service
- Sets the root password securely
- Creates and deploys `.my.cnf` file via Jinja2 for secure passwordless access

---

## 🚀 How to Deploy

### 1. Clone the Project

```bash
git clone https://github.com/EhabAshrafMu/Lamp_stack_depolyment_using_Ansible_Roles.git
cd Lamp_stack_depolyment_using_Ansible_Roles
```

### 2. Define Inventory

Edit `inventory/hosts`:

```ini
[webservers]
webserver01 ansible_host=<ip of the front-end>

[dbservers]
dbserver01 ansible_host=<ip of the DataBase>
```

### 3. Customize Variables

Edit the files in `group_vars/`:

- `group_vars/dbservers.yaml`:
  ```yaml
  mysql_root_password: yourpassword
  ```

- `group_vars/webservers.yaml`:
  ```yaml
  apache_server_name: your.domain.com
  apache_document_root_directory: /var/www/html
  ```

### 4. Run the Playbook

```bash
ansible-playbook -i inventory/hosts playbook.yaml
```

---

## ✅ Validation Steps

### On the Web Server:
Access in your browser:
```
http://webserver01/phpinfo.php
```

You should see the PHP info page if Apache and PHP are properly installed.

### On the DB Server:
Login to MySQL:
```bash
mysql -u root -p
```

Or if `.my.cnf` is deployed:
```bash
mysql
```

---

## 📜 Notes

- Ensure SELinux and firewalld are configured appropriately or disabled for testing.
- You can secure `.my.cnf` by placing it in `/root` and setting mode `0600`.

---


