# ===============================
# Stage 1 – Build React + Vite (TypeScript)
# ===============================
FROM node:20-alpine AS build

# Set working directory inside container
WORKDIR /app

# Copy dependency files first for caching
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the source code
COPY . .

# Build the optimized production bundle (output -> /app/dist)
RUN npm run build



# ===============================
# Stage 2 – Serve with Nginx
# ===============================
FROM nginx:alpine

# Copy the built static files from the previous stage
COPY --from=build /app/dist /usr/share/nginx/html

# Remove the default Nginx configuration and replace it with ours
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 **inside the container**
# (host will map to 3110:80 via docker-compose)
EXPOSE 80

# Run Nginx in foreground mode
CMD ["nginx", "-g", "daemon off;"]
