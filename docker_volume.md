# Docker Volumes: A Deep Dive
Docker volumes are a feature provided by Docker to manage persistent data generated and used by Docker containers. They enable containers to store and share data reliably across restarts or even across containers.

# Why Docker Volumes?
Containers are designed to be ephemeral, meaning their file systems are reset when stopped or deleted. This behavior is unsuitable for scenarios where data persistence is critical, such as databases, logs, or application configurations. Docker volumes solve this problem by providing a way to store data outside the container's writable layer

# Practical Scenerio
#suppose we have sql container running in my host machine and as we know containers are designed to be ephemeral in nature so by any chance if our container gone down or stop, all the data of our sql container will also be lost.

#So to overcome from this problem , we have docker volumes.

# firstly we need to create a directory in our host system 
for e.g (1) 
```
   mkdir mysql_volume
```
#Now from the sql image , we have in our docker engine , we will run this command:

#(2)
```
  docker run -d --name mysql-container -v /home/ubuntu/mysql_volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:latest
```

#-v /home/ubuntu/mysql_volume:/var/lib/mysql =: this command is used to map the directory we created with the existing docker volume for persistent data storage with -v flag

#(3)
```
   docker exec -it <container id> bash : to enter in mysql container
```

#(4)  
```
   mysql -u root -p : then enter your pass word (root)
```

#now inside the sql container run few commands :
```
  (1): create database kyc_devops;
  (2): use kyc_devops;
```

#(3):
 ```sql
     CREATE TABLE messages (
         id INT AUTO_INCREMENT PRIMARY KEY,
         message TEXT
     );
 ```

#(4):
```
   insert into messages (message) values ("kyc submitted"); : run this command 4-5 times to create multiple user in database.
```

#(5):
```
   select * from messages; : we will be able to see a dates with 4-5 lines of id and message.
```

#now exit from this container and now stop or remove this container , we will still be able to see a directory name kyc_devops in my mysql_volume folder.

#now that something is called as docker volumes.

#if we again run a container with same above mention command and repeat the whole process and enter in our sql container and run the command #show databases;

#we can see that there is database "kyc_devops" in our new docker container.

#Types of docker volumes:

Docker supports two types of volumes: #unnamed (or anonymous) volumes and #named volumes. Both serve the purpose of persisting data beyond the lifecycle of a container, but they differ in management, usability, and use cases.

# Difference Between Unnamed (Anonymous) Volumes and Named Volumes:
Docker supports two types of volumes: unnamed (or anonymous) volumes and named volumes. Both serve the purpose of persisting data beyond the lifecycle of a container, but they differ in management, usability, and use cases.

# 1.Unnamed (Anonymous) Volumes
An unnamed or anonymous volume is created automatically by Docker when you specify a volume mount without explicitly naming the volume. It is identified by a unique, randomly generated name.
# Characteristics of Unnamed Volumes
No Explicit Name: Docker assigns a unique identifier to the volume.

Use Case: Primarily for quick, temporary setups where you don't need to reference the volume again.

Harder to Manage: Since they lack a human-readable name, tracking and managing unnamed volumes is more challenging.

Default Behavior: If you specify a volume without providing a name, Docker creates an anonymous volume.

Lifecycle: The volume remains even after the container is removed unless explicitly cleaned up

# 2. Named Volumes
A named volume is explicitly created and referred to by a specific name, making it easier to identify and manage.

Characteristics of Named Volumes:

Human-Readable Name: You assign a specific name to the volume, making it easier to reference and manage.

Use Case: Suitable for production environments and reusable setups where the same volume is used across multiple containers.

Easier Management: Named volumes are straightforward to locate, inspect, and remove.

Lifecycle: Independent of container lifecycle. Even if a container using the volume is deleted, the volume remains
