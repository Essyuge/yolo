# Build stage
FROM node:14-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json to install dependencies
COPY package*.json ./

# Install npm dependencies
RUN npm install

# Copy the rest of the client application code to the working directory
COPY . .



EXPOSE 3000

CMD ["serve", "-s", "build", "-l", "3000"]

