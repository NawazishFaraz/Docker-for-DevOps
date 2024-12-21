# Multi-Stage Dockerfile

A **multi-stage Dockerfile** allows you to build Docker images efficiently by using multiple stages. It creates intermediate images for building or compiling applications and copies only the necessary artifacts into the final lightweight image.

## Why Use a Multi-Stage Dockerfile?

1. **Smaller Final Images**: 
   - Only the necessary files and dependencies are included in the final image, reducing its size.

2. **Cleaner Build Process**: 
   - Intermediate stages can include tools and libraries required for building the application without including them in the final image.

3. **Security and Efficiency**:
   - Unused build tools and dependencies are left behind, minimizing vulnerabilities and improving performance.

---

## Example: Multi-Stage Dockerfile for a Flask Application

### Regular (Single-Stage) Dockerfile:
```dockerfile
# Use an official Python runtime as the base image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# install required packages for system
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y gcc default-libmysqlclient-dev pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Copy the requirements file into the container
COPY requirements.txt .

# Install app dependencies
RUN pip install mysqlclient
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application code
COPY . .

# Specify the command to run your application
CMD ["python", "app.py"]
```

### Multi-Stage Dockerfile:
```dockerfile
# ------------------- Stage 1: Build Stage ------------------------------
FROM python:3.9 AS builder

WORKDIR /app

# Install build dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends gcc default-libmysqlclient-dev pkg-config && \
    rm -rf /var/lib/apt/lists/*

# Copy and install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# ------------------- Stage 2: Final Stage ------------------------------
FROM python:3.9-slim

WORKDIR /app

# Install runtime dependencies only
RUN apt-get update && \
    apt-get install -y --no-install-recommends libmariadb3 && \
    rm -rf /var/lib/apt/lists/*

# Copy dependencies and application code from the builder stage
COPY --from=builder /usr/local/lib/python3.9/site-packages/ /usr/local/lib/python3.9/site-packages/
COPY . .

CMD ["python", "app.py"]

```

# Stages of a Multi-Stage Dockerfile
### Build Stage:

Uses a larger base image (e.g., python:3.9) with all tools needed for compiling and building the application.

Installs build dependencies and application libraries.

Generates the final build artifact (e.g., compiled binaries or Python dependencies).

### Final Stage:

Uses a lightweight base image (e.g., python:3.9-slim) suitable for production

Installs only runtime dependencies.

Copies the necessary build artifacts (from the build stage) and application code.

Defines the entry point or command to run the application.

# Advantages of this Approach:

### Separation of Concerns:
The build environment (with heavy dependencies like gcc) is separate from the runtime environment, which remains lightweight.

### Smaller Final Image:
The final image (python:3.9-slim) contains only the runtime libraries and application code, making it significantly smaller.

### Security and Efficiency:
The runtime image avoids including unnecessary build tools, reducing the attack surface and improving performance.






