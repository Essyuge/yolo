#   Overview
This file outlines a step-by-step procedure of how this
application was containerized using Docker 

#   Requirements
Make sure that you have the following installed:
- [Docker](https://docs.docker.com/engine/install/) 

#   Containerize the client/frontend
- Navigate to the client directory

  `cd client`

- Create a Dockerfile

  `touch Dockerfile`

- Configure the Dockerfile using a multi-stage build
  to ensure that the resultant image is lightweight

```
# Build stage
FROM node:14-alpine3.16 as build

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json .

# Install npm dependencies
RUN npm install

# Copy the rest of the client application code to the working directory
COPY . .


# Build the application and  remove development dependencies
RUN npm run build && \
    npm prune --production

# Production stage
FROM node:16-alpine3.16 as production-stage

# Set the working directory inside the container
WORKDIR /app

# Copy the built app from the build stage to the production container
COPY --from=build /app /app

# Expose port 3000 to communicate with the outside world
EXPOSE 3000

# Command to start the application
CMD ["npm", "start"]
```

- Create a .dockerignore file

  `touch .dockerignore`

- In the .dockerignore file, specify which files 
  to exclude when building the Docker image

```
node_modules

```

#   Containerize the backend
- Navigate to the backend directory

  `cd backend`

- Create a Dockerfile
- 
  `touch Dockerfile`

- Configure the Dockerfile using a multi-stage build
  to ensure that the resultant image is lightweight

```
FROM node:14-alpine3.16

# Set the working directory to /app inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install npm dependencies
RUN npm install

# Copy the entire of the application code to the working directory
COPY . .


# Expose the port on which the Node.js application will run
EXPOSE 5000


# Command to start the Node.js application
CMD ["npm", "start"]


```


- Create a .dockerignore file

  `touch .dockerignore`

- In the .dockerignore file, specify which files 
  to exclude when building the Docker image

```
node_modules

```

#   Configure the Microservice Architecture using Docker Compose
- Navigate to the root directory of the repository
  and create a docker-compose.yaml file

  `touch docker-compose.yaml`

- Configure the docker-compose.yaml file as follows

```
version: '3.8'

services:
  # Backend service configuration
  backend:
    build: ./backend  # Build the backend service using the Dockerfile in the ./backend directory
    image: estheruge/backend-service:v1.0.0  # Tag the built image with a specific version
    container_name: backend_container  # Name of the backend container
    environment:
      - MONGODB_URI=mongodb://mongo:27017/yolomy  # Set the MongoDB connection URI for the backend
    ports:
      - "5000:5000"  # Map port 5000 of the container to port 5000 on the host
    networks:
      - custom_network  # Attach backend service to the custom network
    depends_on:
      - mongo  # Ensure MongoDB service is started before the backend service

  # Client service configuration
  client:
    build: ./client  # Build the client service using the Dockerfile in the ./client directory
    image: estheruge/client-service:v1.0.0  # Tag the built image with a specific version
    container_name: client_container  # Name of the client container
    environment:
      - NODE_ENV=production  # Set Node.js environment to production
    ports:
      - "3000:3000"  # Map port 3000 of the container to port 3000 on the host
    depends_on:
      - backend  # Ensure backend service is started before the client service
    networks:
      - custom_network  # Attach client service to the custom network
    stdin_open: true  # Keep stdin open so container can accept input
    tty: true  # Allocate a pseudo-TTY for the container, useful for debugging

  # MongoDB service configuration
  mongo:
    image: mongo:latest  # Use the latest MongoDB Docker image
    container_name: mongo_container  # Name of the MongoDB container
    networks:
     - custom_network  # Attach MongoDB service to the custom network
    ports:
      - "27017:27017"  # Map port 27017 of the container to port 27017 on the host
    volumes:
      - mongo_data:/data/db  # Mount a volume named 'mongo_data' for MongoDB data persistence

networks:
  custom_network:
    driver: bridge  # Use the bridge network driver for the custom network

volumes:
  mongo_data:
    driver: local  # Use the local volume driver for 'mongo_data' volume


```


#   Ensure connectivity between the backend and the database containers
- Navigate to the server.js file in the backend folder
  and change the "mongodb_url" to point to your mongodb
  container instead of "localhost". In my case it was as follows

  ```
  // Connecting to the Database
  let mongodb_url = 'mongodb://127.0.0.1';
  dbName = 'yolomy';
  ```
 

  ```
  environment:
      - MONGODB_URI=mongodb://mongo:27017/yolomy  # Set the MongoDB connection URI for the backend

  ```

#   Build and start your microservices using Docker Compose Up
- Navigate to the root folder of the repository (where your
  docker-compose.yml file is located) and initialize your
  microservice application using the following command

  `sudo docker compose up`
  
#   Stop the application using Docker Compose Down
- In the same terminal where you launched your microservices,
  press "ctrl+c" to stop the application and terminate it using
  following command

  `sudo docker compose down`