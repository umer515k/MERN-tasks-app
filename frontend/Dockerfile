# Step 1: Build the frontend using Node.js
FROM node:18 AS build

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the frontend code
COPY . .

# Build the React app
RUN npm run build

# Step 2: Set up the NGINX server
FROM nginx:alpine

# Copy the Nginx config file to the container
COPY nginx.conf /etc/nginx/nginx.conf

# Remove the default Nginx index page
RUN rm -rf /usr/share/nginx/html/*

# Copy the built React app to Nginx's default folder
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 for the Nginx server
EXPOSE 80

# Run Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
