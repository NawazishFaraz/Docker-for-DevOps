# Docker Compose

Docker Compose is a tool that simplifies the deployment and management of multi-container Docker applications. It uses a YAML file to define services, networks, and volumes required for your application, enabling you to manage everything with a single command.

---

## **Table of Contents**
1. [What is Docker Compose?](#what-is-docker-compose)
2. [Benefits of Docker Compose](#benefits-of-docker-compose)
3. [How to Use Docker Compose](#how-to-use-docker-compose)
4. [Real-Life Examples](#real-life-examples)
5. [Common Commands](#common-commands)
6. [Conclusion](#conclusion)

---

## **What is Docker Compose?**
Docker Compose is designed for defining and running multi-container applications. With Compose, you define all services your application needs in a single `docker-compose.yml` file. Then, you use simple commands to build, start, and manage the entire application stack.

---

## **Benefits of Docker Compose**
1. **Simplified Deployment**: Manage complex multi-container applications with a single configuration file.
2. **Portability**: Easily share and replicate environments using the `docker-compose.yml` file.
3. **Networking**: Automatically sets up networks for containers to communicate.
4. **Scalability**: Scale services up or down with a single command.
5. **Consistency**: Create consistent environments across development, testing, and production.

---

## **How to Use Docker Compose**

### **1. Define the Application in `docker-compose.yml`**
The `docker-compose.yml` file specifies services, networks, and volumes.

Example:
```yaml
version: "3.8"

services:
  web:
    image: nginx
    ports:
      - "80:80"

  app:
    build:
      context: ./app
    ports:
      - "5000:5000"
    depends_on:
      - db

  db:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

### **2. Start the Application**
Run the following command to build and start all services defined in the `docker-compose.yml` file:
```bash
docker-compose up
```

### **3. Manage Services**
- Stop all services:
  ```bash
  docker-compose down
  ```
- Scale a service:
  ```bash
  docker-compose up --scale app=3
  ```

---

## **Real-Life Examples**

### **Example 1: Flask with MYSQL**
Set up a Python Flask app connected to a Mysql database.

#### `docker-compose.yml`
```yaml
version: "3.8"

services:
  mysql:
    image: mysql:5.7
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: devops
      MYSQL_USER: admin
      MYSQL_PASSWORD: admin
    volumes:
      - ./mysql-data:/var/lib/mysql
      - ./message.sql:/docker-entrypoint-initdb.d/message.sql  
    networks:
      - twotier
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-uroot", "-proot"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 60s

  flask-app:
    build:
      context: .
    container_name: flask-app
    ports:
      - "5000:5000"
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: root
      MYSQL_DB: devops
    depends_on:
      - mysql
    networks:
      - twotier
    restart: always
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://localhost:5000/health || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 30s

networks:
  twotier:
```

Explanation of the Parameters:

test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-uroot", "-proot"]

This is the command executed to check the health of the MySQL container.

mysqladmin is a command-line tool provided by MySQL for administrative tasks.

ping checks whether the MySQL server is alive and responding to requests.

-h localhost: Specifies the host to connect to (in this case, localhost within the container).

-u root: Specifies the MySQL root user.

-p root: Specifies the password for the root user. Adjust if using different credentials

---

## **Common Commands**
- **Start services**:
  ```bash
  docker-compose up
  ```
- **Stop services**:
  ```bash
  docker-compose down
  ```
- **View running services**:
  ```bash
  docker-compose ps
  ```
- **Scale services**:
  ```bash
  docker-compose up --scale <service>=<count>
  ```
- **Rebuild services**:
  ```bash
  docker-compose up --build
  ```

---

## **Conclusion**
Docker Compose simplifies multi-container application management by providing a single configuration file for all components. Whether you're working on a small project or a complex microservices architecture, Compose can help streamline your development and deployment workflows.

Start exploring Docker Compose today to build robust, scalable applications with ease!

