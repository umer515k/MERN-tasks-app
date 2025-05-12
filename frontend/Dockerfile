# Stage 1: Build the React application
FROM node:18 AS build

# Set working directory
WORKDIR /app

# Copy package files first for better caching
COPY package*.json ./
COPY tailwind.config.js ./
COPY postcss.config.js ./

# Install dependencies (including devDependencies)
RUN npm install --legacy-peer-deps

# Copy all source files
COPY . .

# Build the application with specific environment vars
ENV NODE_ENV=production
RUN npm run build

# Fix CSS file references in index.html
RUN sed -i 's/\/static\/\(css\|js\)\/[^"]*/\/static\/\1\/main/g' /app/build/index.html

# Stage 2: Nginx server setup
FROM nginx:alpine

# Remove default nginx configuration
RUN rm -rf /etc/nginx/conf.d/default.conf

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Clean default static files
RUN rm -rf /usr/share/nginx/html/*

# Copy built assets from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Rename hashed files to main.css/main.js
RUN find /usr/share/nginx/html/static/css -type f -name "*.css" -exec mv {} /usr/share/nginx/html/static/css/main.css \;
RUN find /usr/share/nginx/html/static/js -type f -name "*.js" -exec mv {} /usr/share/nginx/html/static/js/main.js \;

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]