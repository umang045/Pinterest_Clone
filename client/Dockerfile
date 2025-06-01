# Base image
FROM node:18-alpine as builder

# Set working directory
WORKDIR /app

# Copy package files
COPY package.*json  ./

# Install dependencies
RUN npm install

# Copy rest of the code
COPY . .

# Run build
RUN npm run build

# ---- Production image ----
FROM nginx:alpine

# Copy build output to NGINX's web folder
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy default nginx config if needed (optional)
# COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
