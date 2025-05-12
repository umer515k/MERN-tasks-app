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

# Copy custom nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Remove default static files
RUN rm -rf /usr/share/nginx/html/*

# Copy built files
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
