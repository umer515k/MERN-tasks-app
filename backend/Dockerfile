# Use official Node.js image as base image
FROM node:18

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the backend port (in this case, 5000)
EXPOSE 5000

# Command to run the application
CMD ["node", "app.js"]
