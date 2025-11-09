# ---------- Stage 1: Build ----------
FROM node:18-alpine AS build
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --legacy-peer-deps

# Copy all app files
COPY . .

# Fix OpenSSL issue with Webpack (Node 17+)
ENV NODE_OPTIONS=--openssl-legacy-provider

# Build the production-ready static files
RUN npm run build

# ---------- Stage 2: Serve ----------
FROM nginx:alpine

# Copy the build output to Nginx web directory
COPY --from=build /app/build /usr/share/nginx/html

# Expose Nginx port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
