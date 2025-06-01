# client/Dockerfile

# Build stage: use Node.js official image to build the app
FROM node:18 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json (if any) to install dependencies
COPY package*.json ./

# Install dependencies based on package-lock.json for reproducible builds
RUN npm ci

# Copy all source files into the working directory
COPY . .

# Build the Vite React app for production (output in /app/dist)
RUN npm run build

# Production stage: use lightweight nginx image to serve the built app
FROM nginx:alpine

# Copy built files from the builder stage to nginx's public folder
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose port 80 (default HTTP port)
EXPOSE 80

# Start nginx in the foreground (required for container to keep running)
CMD ["nginx", "-g", "daemon off;"]
