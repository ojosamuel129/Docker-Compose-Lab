# WordPress Deployment with Docker Compose

An end-to-end guide demonstrating how to deploy a full-stack WordPress website paired with a MySQL database using Docker Compose.

---

## 📌 Introduction

This project demonstrates how to deploy a WordPress website with a MySQL database using Docker Compose. Docker Compose makes it easy to define, configure, and manage multiple containers from a single `compose.yml` file.

In this project, you will deploy two containers:

* **MySQL** – Stores the WordPress database.
* **WordPress** – Hosts the website and connects to the MySQL database.

Docker Compose automatically creates a network for both containers, allowing them to communicate securely without additional configuration.

---

## 🎯 Objectives

* Create a Docker Compose project
* Deploy a multi-container application
* Configure a MySQL database
* Connect WordPress to MySQL
* Use Docker volumes for persistent storage
* Verify running containers
* View container logs
* Stop and remove the application stack

---

## 📋 Prerequisites

Before starting, ensure you have the following installed:

* **Docker Desktop** (Windows / macOS)
* **Docker Engine** (Linux)
* **Docker Compose** (included with Docker Desktop)

Verify your installation:

```bash
docker --version
docker compose version

```

---

## 📂 Project Structure

```text
my-wordpress-site/
└── compose.yml

```

---

## 🛠️ Step-by-Step Setup

### Step 1: Create the Project Directory

```bash
mkdir my-wordpress-site
cd my-wordpress-site

```

---

### Step 2: Create the Compose File

Create a file named `compose.yml`:

```yaml
services:
  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: supersecretpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpressuser
      MYSQL_PASSWORD: wordpresspassword
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpressuser
      WORDPRESS_DB_PASSWORD: wordpresspassword
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wp_data:/var/www/html

volumes:
  db_data:
  wp_data:

```

#### Architecture Breakdown

* **`db`**
* Uses the official `mysql:8.0` image.
* Creates a `wordpress` database and dedicated user.
* Stores database files in a persistent Docker volume.
* Automatically restarts if the container stops.

* **`wordpress`**
* Uses the latest official `wordpress` image.
* Waits for the database service to start (`depends_on`).
* Maps host port `8080` to container port `80`.
* Connects to MySQL using environment variables.
* Stores website files in a persistent Docker volume.

* **`volumes`**
* `db_data`: Persists MySQL database files.
* `wp_data`: Persists WordPress website files.

---

### Step 3: Start the Application

```bash
docker compose up -d

```

**What this does:**

1. Downloads the required Docker images.
2. Creates the Docker network.
3. Creates persistent volumes.
4. Starts both MySQL and WordPress containers in detached mode.

**Expected output:**

```text
Creating network "my-wordpress-site_default"
Creating volume "my-wordpress-site_db_data"
Creating volume "my-wordpress-site_wp_data"
Creating my-wordpress-site-db-1
Creating my-wordpress-site-wordpress-1

```

---

### Step 4: Verify the Running Containers

```bash
docker compose ps

```

**Example output:**

```text
NAME                              IMAGE               STATUS          PORTS
my-wordpress-site-db-1            mysql:8.0          Up
my-wordpress-site-wordpress-1     wordpress:latest   Up              0.0.0.0:8080->80/tcp

```

---

### Step 5: Access the WordPress Site

Open your browser and navigate to:
👉 **`http://localhost:8080`**

The WordPress installation page will appear, prompting you to select a language, enter site details, and set up an administrator account

---

### Step 6: View Container Logs

```bash
docker compose logs --tail 20

```

Displays the last 20 log entries from both containers to verify successful connections:

```text
db-1         | MySQL init process done. Ready for start up.
wordpress-1  | WordPress not found in /var/www/html - copying now...
wordpress-1  | Apache/2.4 configured — resuming normal operations

```

---

### Step 7: Stop and Remove the Application

```bash
docker compose down -v

```

**What this does:**

* Stops and removes both containers.
* Removes the Docker network.
* Deletes the persistent volumes (`db_data` and `wp_data`).

> ⚠️ **Warning:** Including `-v` deletes the persistent volumes along with all WordPress content and database data.

---

## 📚 Quick Commands Reference

| Command | Description |
| --- | --- |
| `docker compose up -d` | Start all services in detached mode |
| `docker compose ps` | List running services |
| `docker compose logs --tail 20` | Display the last 20 log entries |
| `docker compose down -v` | Stop services and remove containers, networks, and volumes |

---

## 🧠 Key Concepts & Learning Outcomes

By completing this project, you will learn and apply:

* **Docker Compose**: Orchestrating multi-container applications.
* **Service Dependencies**: Managing startup order with `depends_on`.
* **Networking & Environment Variables**: Secure inter-container communication.
* **Data Persistence**: Preserving state using Docker Volumes.
* **Lifecycle Management**: Building, inspecting, and dismantling stacks safely.

---

## 👤 Author

**Samuel Ojo**
**Cloud & DevOps Engineer**

**GitHub:** [@ojosamuel129](https://github.com/ojosamuel129)

---

