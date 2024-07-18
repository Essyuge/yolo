# Set base image
FROM node:14-alpine3.16

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .


# Expose the port on which the Node.js application will run
EXPOSE 5000


# Command to run the Node.js application
CMD ["npm", "start"]

