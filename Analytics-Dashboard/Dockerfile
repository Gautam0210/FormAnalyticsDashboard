# Use an official Node runtime as the base image
FROM node:20-alpine

# Set the working directory in the container
WORKDIR /app

# Install global dependencies
RUN npm install -g npm@latest vite@latest

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install project dependencies
RUN npm ci

# Copy the rest of the project files
COPY . .

# Build the project (optional, uncomment if you want to build before running)
# RUN npm run build

# Create a non-root user for security
RUN addgroup -S appuser && adduser -S appuser -G appuser
USER appuser

# Expose the port Vite uses by default
EXPOSE 5173

# Command to run the development server
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]



# # Use an official Node runtime as the base image
# FROM node:20-alpine

# # Set the working directory in the container
# WORKDIR /Analytics-Dashboard

# # Copy package.json and package-lock.json (if available)
# COPY package*.json ./

# # Install project dependencies
# RUN npm install

# # Copy the rest of the project files
# COPY . .

# # Expose the port Vite uses by default
# EXPOSE 5173

# # Command to run the development server
# CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
