# Stage 1: Build Vue.js App
FROM node:18 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy only package.json & package-lock.json for better caching
COPY frontend/package.json frontend/package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the frontend app
COPY frontend/ .

# Build the Vue.js app
RUN npm run build

# Stage 2: Nginx Server to Serve Vue.js
FROM nginx:latest AS base

# Set working directory
WORKDIR /usr/share/nginx/html

# Remove default Nginx static files
RUN rm -rf ./*

# Copy built Vue.js app from the builder stage
COPY --from=builder /app/public .

# Copy custom Nginx configuration
COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf

# Stage 3: Final Image
FROM base AS final

# Expose port 80 for Nginx
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
