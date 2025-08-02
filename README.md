GradeQSlot - Complete Setup Guide
A modern web application built with Laravel (backend), Vuetify (frontend), and MySQL database, all containerized with Docker for easy development and deployment.
🛠️ Technology Stack

Backend: Laravel 12.x with PHP 8.4
Frontend: Vuetify 3.9.x with Vue 3 and Vite
Database: MySQL 8.4 LTS
Communication: Axios for API calls
Containerization: Docker & Docker Compose

📋 Prerequisites
Before you begin, make sure you have the following installed on your system:

Docker (version 20.0 or higher)
Docker Compose (version 2.0 or higher)
Git

To verify your installations:
bashdocker --version
docker-compose --version
git --version
📁 Project Structure
Your project should be organized like this:
GradeQSlot/
├── docker-compose.yml          # Main Docker orchestration file
├── backend/                    # Laravel backend
│   ├── Dockerfile             # Backend Docker configuration
│   ├── app/
│   ├── config/
│   ├── database/
│   ├── routes/
│   ├── composer.json
│   ├── .env.example
│   └── ... (other Laravel files)
├── frontend/                   # Vuetify frontend
│   ├── Dockerfile             # Frontend Docker configuration
│   ├── src/
│   ├── package.json
│   ├── vite.config.js
│   └── ... (other Vuetify files)
├── nginx/                      # Nginx configuration (optional)
│   └── nginx.conf
└── README.md                   # This file
🚀 Step-by-Step Setup
Step 1: Clone Your Repositories
First, create the main project directory and clone your existing repositories:
bash# Create main project directory
mkdir GradeQSlot
cd GradeQSlot

# Clone your Laravel backend repository
git clone <your-laravel-repository-url> backend

# Clone your Vuetify frontend repository  
git clone <your-vuetify-repository-url> frontend
Alternative: If you're starting fresh, you can create new projects:
bash# For Laravel backend
cd backend
composer create-project laravel/laravel . "^12.0"

# For Vuetify frontend
cd frontend
npm create vuetify@latest . -- --name gradeqslot-frontend
Step 2: Add Docker Configuration Files
Copy the Docker configuration files to your project:

docker-compose.yml → Place in the root GradeQSlot/ directory
backend/Dockerfile → Place in the backend/ directory
frontend/Dockerfile → Place in the frontend/ directory
nginx/nginx.conf → Create nginx/ directory and place the config file there

Step 3: Configure Laravel Backend
Navigate to your backend directory and set up the environment:
bashcd backend

# Copy environment file
cp .env.example .env

# Edit the .env file with the following database settings:
Open .env file and update these settings:
envAPP_NAME=GradeQSlot
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=gradeqslot
DB_USERNAME=laravel_user
DB_PASSWORD=laravel_password
Step 4: Configure Vuetify Frontend
Navigate to your frontend directory and set up the environment:
bashcd frontend

# Create environment file for API configuration
echo "VITE_API_URL=http://localhost:8000/api" > .env
Optional: If axios is not installed in your Vuetify project:
bashnpm install axios
Step 5: Build and Start the Application
From the root GradeQSlot/ directory:
bash# Build and start all containers
docker-compose up --build

# Or run in background (detached mode)
docker-compose up --build -d
First time setup: The build process will take a few minutes as it downloads images and installs dependencies.
Step 6: Initialize Laravel Database
Once the containers are running, set up your Laravel database:
bash# Generate Laravel application key
docker-compose exec backend php artisan key:generate

# Run database migrations
docker-compose exec backend php artisan migrate

# Optional: Seed database with sample data
docker-compose exec backend php artisan db:seed
🌐 Access Your Application
After successful setup, you can access:

Frontend (Vuetify): http://localhost:3000
Backend API (Laravel): http://localhost:8000
Database (MySQL): localhost:3306
Nginx (Production): http://localhost:80

📝 Useful Commands
Container Management
bash# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# Restart specific service
docker-compose restart backend

# View logs for all services
docker-compose logs -f

# View logs for specific service
docker-compose logs -f frontend
Backend (Laravel) Commands
bash# Access backend container shell
docker-compose exec backend bash

# Run Artisan commands
docker-compose exec backend php artisan migrate
docker-compose exec backend php artisan make:controller ExampleController
docker-compose exec backend php artisan route:list

# Install new Composer packages
docker-compose exec backend composer require package-name

# Clear application cache
docker-compose exec backend php artisan cache:clear
docker-compose exec backend php artisan config:clear
Frontend (Vuetify) Commands
bash# Access frontend container shell
docker-compose exec frontend sh

# Install new npm packages
docker-compose exec frontend npm install package-name

# Run build for production
docker-compose exec frontend npm run build

# Check package versions
docker-compose exec frontend npm list
Database Commands
bash# Access MySQL shell
docker-compose exec mysql mysql -u laravel_user -p gradeqslot

# Backup database
docker-compose exec mysql mysqldump -u laravel_user -p gradeqslot > backup.sql

# Import database backup
docker-compose exec -i mysql mysql -u laravel_user -p gradeqslot < backup.sql
🔧 Development Workflow

Make code changes: Edit files in ./backend or ./frontend directories
Automatic sync: Changes are automatically reflected in containers
Hot reload: Both Laravel and Vuetify support hot reloading
API testing: Use http://localhost:8000/api for testing endpoints
Frontend development: Vuetify runs with Vite for instant updates

🚨 Troubleshooting
Common Issues and Solutions
Port already in use:
bash# Check what's using the port
sudo lsof -i :3000
sudo lsof -i :8000

# Kill the process or change ports in docker-compose.yml
Permission issues:
bash# Fix Laravel storage permissions
docker-compose exec backend chmod -R 775 storage bootstrap/cache
docker-compose exec backend chown -R www-data:www-data storage bootstrap/cache
Database connection issues:
bash# Check if MySQL container is running
docker-compose ps

# Restart database container
docker-compose restart mysql

# Check database logs
docker-compose logs mysql
Frontend not loading:
bash# Check if node_modules is properly installed
docker-compose exec frontend npm install

# Restart frontend container
docker-compose restart frontend
Clear everything and start fresh:
bash# Stop all containers and remove volumes
docker-compose down -v

# Remove all containers and images
docker-compose down --rmi all

# Rebuild everything
docker-compose up --build
📦 Production Deployment
For production deployment:

Update environment variables in .env files
Use Nginx configuration included in the setup
Set APP_ENV=production in Laravel .env
Build optimized frontend:
bashdocker-compose exec frontend npm run build

Optimize Laravel:
bashdocker-compose exec backend php artisan config:cache
docker-compose exec backend php artisan route:cache
docker-compose exec backend php artisan view:cache


🆘 Getting Help
If you encounter issues:

Check the logs: docker-compose logs -f [service-name]
Verify file structure: Ensure all files are in correct directories
Check permissions: Ensure Docker has access to project files
Review environment variables: Verify .env files are configured correctly
Restart containers: Sometimes a simple restart fixes issues

📄 License
[Add your license information here]

Happy coding! 🎉
For additional support or feature requests, please refer to the project documentation or contact the development team.