# Use Node.js 14 as the base image
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

