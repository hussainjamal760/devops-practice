📌 Session Overview
In this session, we covered:
✅ Docker Fundamentals – Containers, Images, Docker Daemon, Docker CLI, and Docker Registry
✅ Working with Docker – Writing a Dockerfile, building images, running containers
✅ Docker Compose – Multi-container applications, networking, and volumes
✅ Container Orchestration – What it is and why we need it
✅ Hands-on Examples – Running Node.js with MongoDB using Docker and Docker Compose

📌 Figma Board for Session 3: View Here 📌 Youtube Video Link Session 3: View Here

🛠️ 1. Introduction to Docker
Docker is a containerization platform that allows developers to package applications with all dependencies into lightweight, portable containers.

Why Docker?
✔ Solves "It works on my machine" problem
✔ Lightweight and fast compared to VMs
✔ Easy to scale and deploy
✔ Works across different environments

🔹 Key Docker Components
Component	Description
Docker Engine (Daemon)	Runs containers, manages images, networks, and volumes.
Docker Client (CLI)	Used to communicate with the Docker Daemon.
Docker Image	A template containing the app and dependencies.
Docker Container	A running instance of an image.
Docker Registry	Stores and distributes images (e.g., Docker Hub, AWS ECR).
📝 2. Writing a Dockerfile
A Dockerfile defines how to build a Docker image.
Here’s a simple example for a Node.js app:

# Use a lightweight Node.js base image
FROM node:alpine

# Set the working directory inside the container
WORKDIR /nodejs-docker-aws-ecs

# Copy package.json and install dependencies
COPY package.json .
RUN npm install

# Copy the rest of the application files
COPY . .

# Expose port 3000
EXPOSE 3000

# Run the application
CMD ["node", "app.js"]
How to Build and Run This?
docker build -t my-node-app .
docker run -p 3000:3000 my-node-app
Now visit http://localhost:3000 🎯

🛠️ 3. Docker Compose
Docker Compose manages multi-container applications using a docker-compose.yml file.

Why Docker Compose?
✔ Easier multi-container setup
✔ Automatic networking between services
✔ Simplifies managing environment variables and volumes

Example: Node.js with MongoDB
version: "3.8"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - mongo
    environment:
      - MONGO_URL=mongodb://mongo:27017/mydatabase
    networks:
      - app_network

  mongo:
    image: mongo
    restart: always
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    networks:
      - app_network

volumes:
  mongo_data:

networks:
  app_network:
    driver: bridge
How to Run?
docker-compose up -d
Now, app and mongo containers communicate over the app_network.

📂 4. Persistent Storage in Docker
Without volumes, MongoDB loses all data when the container stops.
To test this:

volumes:
  mongo_data:
With volume → Data persists across container restarts
Without volume → Data is lost when the container stops
🔗 5. Docker Networking
By default, Docker creates a bridge network.
Using custom networks like this:

networks:
  app_network:
    driver: bridge
Containers inside the network can communicate via container names
No need to use IP addresses
📦 6. Running Mongo Express
Mongo Express provides a web interface to explore MongoDB.

Add Mongo Express to docker-compose.yml
  mongo-express:
    image: mongo-express
    restart: always
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: secret
      ME_CONFIG_MONGODB_URL: mongodb://admin:secret@mongo:27017/
    ports:
      - "8081:8081"
    networks:
      - app_network
Access it via
http://localhost:8081
Login with:

Username: admin
Password: secret
🚀 7. Introduction to Container Orchestration
Docker works great for running containers, but managing multiple containers at scale requires orchestration tools.

What is Container Orchestration?
Container Orchestration automates deployment, scaling, and management of containerized applications.

Popular Orchestration Tools
Tool	Description
Docker Swarm	Built into Docker, easy to use but limited.
Kubernetes	Industry standard, highly scalable.
AWS ECS	Managed service for running containers on AWS.
📊 8. Orchestration Example (Asia-Specific)
Food Delivery App Example
Imagine a food delivery service like Foodpanda in Asia.

Without Orchestration: A sudden surge of orders crashes the system.
With Orchestration: Kubernetes or ECS automatically scales up to handle demand.
📢 Conclusion
✅ Docker simplifies packaging and running applications
✅ Docker Compose helps manage multi-container apps
✅ Container Orchestration automates scaling and deployment