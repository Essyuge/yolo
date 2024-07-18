# Build stage
FROM node:16-alpine3.16 as build

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json .

# Install dependencies
RUN npm install

# Copy the rest of the client application code to the working directory
COPY . .


#Stage 2: Production stage
FROM node:16-alpine3.16 as production-stage

WORKDIR /app

# Copy the built app from the build stage to the runtime container


COPY --from=build /app /app



# Expose the port used by the app
EXPOSE 3000


# Start the application
CMD ["npm", "start"]

