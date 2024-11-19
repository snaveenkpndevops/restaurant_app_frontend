# Stage 1: Build the Angular app
FROM node:18 AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the Angular project files
COPY . .

# Build the Angular app
RUN npm run build

# Stage 2: Serve the Angular app with Nginx
FROM nginx:stable-alpine

# Copy the built Angular files to Nginx's HTML directory
COPY --from=build /app/dist/frontend /usr/share/nginx/html

# Copy the custom Nginx configuration
#COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 4200 (or your desired port)
#EXPOSE 4200

# Start Nginx server
CMD ["nginx", "-g", "daemon off;"]
