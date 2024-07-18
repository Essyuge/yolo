# YOLO APP

## 1. Base Image Selection

Alpine Linux was chosen as the base image for its lightweight nature and strong security features. Known for minimalism, it ensures smaller Docker image sizes and faster builds. Alpine's wide support and compatibility also make it ideal for efficient containerized applications, prioritizing both performance and security.

## 2. Dockerfile Directives

### Dockerfile Creation and Running Directives

The Dockerfile includes directives to define the build process for each service. Key directives used are:

- **FROM:** Specifies the base image for the service.
- **WORKDIR:** Sets the working directory inside the container.
- **COPY:** Copies files from the host machine to the container.
- **RUN:** Executes commands during the build process.
- **CMD:** Specifies the command to run when the container starts.
- **EXPOSE:** Exposes ports from the container to the host machine.

## 3. Docker-Compose Networking

### Application Port Allocation and Bridge Network Implementation

Docker Compose is used to manage multi-container applications. Networking and port allocation are configured in the `docker-compose.yml` file:

```
version: '3.8'

services:
  # Backend service configuration
  backend:
    build: ./backend
    image: estheruge/backend-service:v1.0.0
    container_name: backend_container
    environment:
      - MONGODB_URI=mongodb://mongo:27017/yolomy
    ports:
      - "5000:5000"
    networks:
      - custom_network
    depends_on:
      - mongo

  # Client service configuration
  client:
    build: ./client
    image: estheruge/client-service:v1.0.0
    container_name: client_container
    environment:
      - NODE_ENV=production
    ports:
      - "3000:3000"
    depends_on:
      - backend
    networks:
      - custom_network
    stdin_open: true
    tty: true

  # MongoDB service configuration
  mongo:
    image: mongo:latest
    container_name: mongo_container
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db
    networks:
      - custom_network

networks:
  custom_network:
    driver: bridge

volumes:
  mongo_data:
    driver: local

```

#### In this setup:

- Ports 5000, 3000, and 27017 are mapped between the host and containers.
- A custom bridge network `custom_network` connects all services for internal communication.

## 4. Docker-Compose Volume Definition and Usage

- Volume Definition and Usage

Volumes ensure persistent data storage. In the `docker-compose.yml`:

```
volumes:
  mongo_data:
    driver: local
```

## 5 .Git Workflow

Git Workflow Used
- Cloning the Repository: Clone the project repository to local machines using the `git clone` command: `https://github.com/Essyuge/yolo.git`.
- Making Changes: Make changes to files locally, including implementing new features, fixing bugs, or making enhancements.
- Committing Changes: After making changes, commit them directly to the master branch using the `git add <filename>` and `git commit -m"commit message"` commands.
- Pushing Changes: Once changes are committed locally, push them to the remote repository using the `git push` command.

## 6. Successful Running of Applications and Debugging Measures

- Deployment and Debugging
- Successful Deployment: Ensure Docker and Docker Compose are installed.
- Debugging Measures: Use:
  - `docker build -t <imagename:version>` to build an image.
  - `docker images` to check images built.
  - `docker ps` to check container status.
  - `docker login` to log in to DockerHub.
  - `docker push <username:image>` to deploy image to DockerHub.
  - `docker-compose down` followed by `docker-compose up` for restarts.
  - `docker-compose logs <service>` for logs.

## 7. Docker Image Tag Naming Standards

- Docker Image Tag Naming Standards
- Images are tagged using semantic versioning (`v1.0.0`):
  - `estheruge/backend-service:v1.0.0`
  - `estheruge/client-service:v1.0.0`


## 8. DockerHub Screenshot

### Screenshot of Deployed Image on DockerHub

![Alt Text](<images/backendimage.png>)

![Alt Text](<images/frontendimage.png>)




