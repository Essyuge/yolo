# Build stage
FROM node:14-alpine as build

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install npm dependencies
RUN npm install

# Copy the rest of the client application code to the working directory
COPY . .

# Build the application
RUN npm run build

# Production stage
FROM node:14-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy built files from the build stage
COPY --from=build /app/build /app/build

# Install the 'serve' package globally
RUN npm install -g serve

# Expose port 3000
EXPOSE 3000

# Command to start the server
CMD ["serve", "-s", "build", "-l", "3000"]

