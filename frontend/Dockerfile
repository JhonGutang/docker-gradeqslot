FROM node:22-alpine

# Set working directory
WORKDIR /app

# Copy package files first for better Docker layer caching
COPY package*.json ./

# Install dependencies
RUN npm install

# Install axios for API communication (if not already in package.json)
RUN npm list axios || npm install axios

# Copy project files
COPY . .

# Expose port
EXPOSE 3000

# Start the development server with host binding
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0", "--port", "3000"]
