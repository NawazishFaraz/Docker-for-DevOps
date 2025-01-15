# Docker Networking

Docker networking enables containers to communicate with each other, the host machine, and the outside world. It provides various networking models to suit different use cases, ranging from simple container-to-container communication to complex multi-host networks.

---

## **Table of Contents**

1. [Overview of Docker Networking](#overview-of-docker-networking)
2. [Types of Docker Networks](#types-of-docker-networks)
3. [Real-Life Examples](#real-life-examples)
4. [Useful Commands](#useful-commands)

---

## **Overview of Docker Networking**

When you run a container, Docker attaches it to a network. By default, Docker creates a bridge network named `bridge` and assigns containers to it unless specified otherwise. Containers can also connect to other types of networks based on specific requirements.

---

## **Types of Docker Networks**

### 1. **Bridge (Default)**
- Containers can communicate with each other on the same bridge network.
- Best for standalone applications.

### 2. **Host**
- The container shares the host machine's network stack.
- Useful when the container needs to use the host's network without isolation.

### 3. **None**
- Disables networking for the container.
- Ideal for security or testing purposes.

### 4. **Overlay**
- Enables communication between containers across multiple hosts.
- Requires Docker Swarm or orchestration tools like Kubernetes.

### 5. **Macvlan**
- Assigns a unique MAC address to the container, making it appear as a physical device on the network.
- Used for legacy applications or advanced networking setups.

---

## **Real-Life Examples**

### **Example 1: Bridge Network for Isolated Containers**

Imagine you have two containers:
1. A backend (mqsql)
   ```bash
   docker run -d --name mysql -e MYSQL_DATABASE=mydb -e MYSQL_ROOT_PASSWORD=admin -v mysql_volume:/var/lib/mysql -p 3306:3306 mysql:latest

-e : environment variable

-v : volume mapping

Run docker ps : mysql container will be running
   
2. A fronted (Flask)
   ```bash
   docker run -d --name flaskapp -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=admin -e MYSQL_DB=mydb -p 5000:5000 flaskapp:late
  
we have configured the env variable of sql container in flask container

#### Steps:
1. Create a bridge network:
   ```bash
   docker network create -d bridge twotier
   ```
-d = driver = bridge 

2. Run the containers on the bridge network:
   ```bash
   docker run -d --name mysql -e MYSQL_DATABASE=mydb -e MYSQL_ROOT_PASSWORD=admin -v mysql_volume:/var/lib/mysql -p 3306:3306 --network twotier mysql:latest 
   ```
   
   ```bash
   docker run -d --name flaskapp -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=admin -e MYSQL_DB=mydb -p 5000:5000 --network twotier flaskapp:latest
   ```
   

3. The frontend container can access the backend using the container name `backend` (e.g., `http://backend:5000`).

---

### **Example 2: Host Network for Performance**

Use the host network to reduce latency for a containerized Nginx server.

#### Steps:
1. Run Nginx with the host network:
   ```bash
   docker run -d --network host nginx
   ```
Access the Nginx server directly via the host's IP and port

## **Useful Commands**

- List all networks:
  ```bash
  docker network ls
  ```

- Inspect a network:
  ```bash
  docker network inspect <network_name>
  ```

- Disconnect a container from a network:
  ```bash
  docker network disconnect <network_name> <container_name>
  ```

- Remove a network:
  ```bash
  docker network rm <network_name>
  ```

---

## **Conclusion**

Docker networking is a powerful feature that simplifies communication between containers, hosts, and external systems. By choosing the appropriate network type and configuration, you can design robust and scalable applications tailored to your use case.

Feel free to explore further by trying out the examples and experimenting with Docker networking commands.
