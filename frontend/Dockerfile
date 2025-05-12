# Stage 1: Build the React application
FROM node:18 AS build

WORKDIR /app

# Copy package files and configs
COPY package*.json ./
COPY tailwind.config.js ./
COPY postcss.config.js ./

# Install dependencies
RUN npm install --legacy-peer-deps

# Copy app source
COPY . .

# Build React app for production
ENV NODE_ENV=production
RUN npm run build

# Stage 2: Serve with NGINX
FROM nginx:alpine

# Remove default static files
RUN rm -rf /usr/share/nginx/html/*

# Copy the custom nginx config file (make sure it's placed correctly in your project)
COPY nginx.conf /etc/nginx/nginx.conf

# Copy built files from build stage to nginx's html directory
COPY --from=build /app/build /usr/share/nginx/html

# Optional: Rename files for better caching strategy (if needed)
RUN find /usr/share/nginx/html/static/css -type f -name "*.css" -exec mv {} /usr/share/nginx/html/static/css/main.css \;
RUN find /usr/share/nginx/html/static/js -type f -name "*.js" -exec mv {} /usr/share/nginx/html/static/js/main.js \;

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
