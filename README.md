# MD Hub - Management Dashboard

A comprehensive management dashboard system with microservices architecture, featuring a Vue.js frontend and NestJS backend, all containerized with Docker and ready for production deployment.

## 🏗️ Project Overview

MD Hub is a full-stack application consisting of:
- **Frontend**: Vue.js 2.x SPA with Vuetify Material Design
- **Backend**: NestJS API with TypeScript
- **Database**: PostgreSQL with Drizzle ORM
- **Message Queue**: RabbitMQ for async processing
- **Cache**: Redis for session storage
- **Containerization**: Multi-stage Docker builds

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose
- Git with submodule support
- Node.js 20+ (for local development)

### Initial Setup

#### 1. Clone with Submodules (Recommended)
```bash
# Clone the entire project with submodules
git clone --recursive https://github.com/Nojorono/md-hub.git
cd md-hub
```

#### 2. Alternative: Clone and Initialize Submodules
```bash
# If you already cloned without submodules
git clone https://github.com/Nojorono/md-hub.git
cd md-hub
git submodule update --init --recursive
```

### Environment Setup

#### 1. Backend Environment
```bash
cd backend
cp env.example .env.docker
# Edit .env.docker with your actual values
```

#### 2. Frontend Environment
```bash
cd frontend
cp env.example .env.docker
# Edit .env.docker with your actual values
```

### Start Complete Application Stack

```bash
# From project root (md-hub/)
docker-compose up --build
```

This starts all services:
- **Frontend**: http://localhost:9000
- **Backend API**: http://localhost:9001
- **PostgreSQL**: localhost:5432 (admin/master123)
- **RabbitMQ**: localhost:5672, Management UI: http://localhost:15672
- **Redis**: localhost:6379 (password: master123)

## 📁 Repository Structure

```
md-hub/
├── .gitmodules              # Submodule configuration
├── docker-compose.yml       # Complete infrastructure stack
├── README.md               # This file
├── backend/                # Backend submodule
│   ├── src/
│   │   ├── modules/       # Feature modules
│   │   │   ├── auth/      # Authentication
│   │   │   ├── user/      # User management
│   │   │   ├── activity/  # Activity tracking
│   │   │   ├── absensi/   # Attendance management
│   │   │   ├── survey/    # Survey functionality
│   │   │   ├── sio/       # SIO (Special Instructions)
│   │   │   ├── outlet/    # Outlet management
│   │   │   ├── regionarea/# Regional area management
│   │   │   ├── program/   # Program management
│   │   │   ├── brand/     # Brand management
│   │   │   ├── callplan/  # Call planning
│   │   │   ├── batch/     # Batch processing
│   │   │   ├── comments/  # Comment system
│   │   │   ├── dashboard/ # Dashboard data
│   │   │   ├── notifications/ # Notification system
│   │   │   ├── reimburseBbm/ # Fuel reimbursement
│   │   │   ├── report/    # Reporting system
│   │   │   └── roles/     # Role-based access control
│   │   ├── common/        # Shared utilities
│   │   ├── config/        # Configuration
│   │   └── main.ts        # Application entry point
│   ├── Dockerfile         # Production Docker image
│   ├── .dockerignore      # Docker build exclusions
│   ├── env.example        # Environment template
│   └── README.md          # Backend documentation
├── frontend/              # Frontend submodule
│   ├── src/
│   │   ├── api/          # API service modules
│   │   ├── components/   # Vue components
│   │   │   ├── base/     # Base UI components
│   │   │   └── ...       # Feature components
│   │   ├── views/        # Page components
│   │   │   ├── auth/     # Authentication pages
│   │   │   ├── dashboard/# Dashboard pages
│   │   │   └── ...       # Other pages
│   │   ├── router/       # Vue Router configuration
│   │   ├── sass/         # Styling with SASS
│   │   ├── locales/      # Internationalization
│   │   └── main.js       # Application entry point
│   ├── Dockerfile        # Production Docker image
│   ├── .dockerignore     # Docker build exclusions
│   ├── env.example       # Environment template
│   └── README.md         # Frontend documentation
└── docs/                 # Additional documentation
    ├── DOCKER_SETUP_DOCUMENTATION.md
    └── QUICK_START_GUIDE.md
```

## 🔧 Working with Submodules

### Understanding Submodules
This repository uses Git submodules to manage the backend and frontend as separate repositories while maintaining them in a unified project structure.

### Submodule Commands

#### Check Submodule Status
```bash
git submodule status
```

#### Update Submodules
```bash
# Update all submodules to latest commits
git submodule update --remote

# Update specific submodules
git submodule update --remote backend
git submodule update --remote frontend
```

#### Work on Individual Submodules
```bash
# Work on backend
cd backend
git checkout development
git pull origin development

# Work on frontend
cd ../frontend
git checkout development
git pull origin development
```

#### Commit Submodule Changes
```bash
# After making changes in submodules
cd backend
git add .
git commit -m "feat: add new feature"
git push origin development

cd ../frontend
git add .
git commit -m "feat: update UI"
git push origin development

# Update main repository with new submodule commits
cd ..
git add backend frontend
git commit -m "chore: update submodules"
git push origin master
```

## 🐳 Docker Configuration

### Complete Stack Deployment
```bash
# Start all services
docker-compose up --build

# Run in background
docker-compose up --build -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

### Individual Service Management
```bash
# Backend only
cd backend
docker build -t backend-app .
docker run -p 9001:9001 backend-app

# Frontend only
cd frontend
docker build -t frontend-app .
docker run -p 9000:9000 frontend-app
```

## 📊 Monitoring & Health Checks

### Service Endpoints
- **Frontend**: http://localhost:9000
- **Backend Health**: http://localhost:9001/health
- **Backend API Docs**: http://localhost:9001/api/docs
- **RabbitMQ Management**: http://localhost:15672 (admin/master123)

### Health Check Commands
```bash
# Application health
curl http://localhost:9001/health

# Database health
docker exec md-postgres pg_isready

# Redis health
docker exec md-redis redis-cli ping

# RabbitMQ health
docker exec md-rabbitmq rabbitmq-diagnostics ping
```

## 🔒 Security Features

### Implemented Security Measures
- ✅ Non-root container execution
- ✅ Alpine Linux base images
- ✅ Proper file permissions
- ✅ Environment variable protection
- ✅ Health monitoring
- ✅ Input validation and sanitization

### Best Practices
- Never commit `.env.docker` files to Git
- Use environment variables for secrets
- Regular security updates
- Image vulnerability scanning

## 🚀 Development Workflow

### Local Development
```bash
# Start core services
yarn dep:up

# Backend development
cd backend
yarn install
yarn dev

# Frontend development
cd frontend
yarn install
yarn dev
```

### Production Deployment
```bash
# Build and deploy
docker-compose up --build -d

# Scale services
docker-compose up --scale backend=3 --scale frontend=2
```

## 📚 Documentation

### Project Documentation
- **Backend Documentation**: See `backend/README.md`
- **Frontend Documentation**: See `frontend/README.md`
- **Docker Setup**: See `docs/DOCKER_SETUP_DOCUMENTATION.md`
- **Quick Start**: See `docs/QUICK_START_GUIDE.md`

### API Documentation
- **Swagger UI**: Available at http://localhost:9001/api/docs when running
- **Health Check**: http://localhost:9001/health

## 🤝 Contributing

### Development Guidelines
1. **Submodule Workflow**: Make changes in individual submodules
2. **Testing**: Ensure all tests pass before committing
3. **Documentation**: Update relevant documentation
4. **Security**: Follow security best practices

### Pull Request Process
1. Create feature branch in appropriate submodule
2. Make changes and test thoroughly
3. Push changes to submodule repository
4. Update main repository with new submodule commits
5. Create pull request for main repository

## 🔄 Version Management

### Submodule Version Control
```bash
# Check current submodule versions
git submodule status

# Update to specific versions
git submodule update --remote backend
git submodule update --remote frontend

# Lock submodule versions
git add backend frontend
git commit -m "chore: update submodule versions"
```

## 📄 License

This project is proprietary and confidential.

---

**Repository**: https://github.com/Nojorono/md-hub  
**Backend**: https://github.com/Nojorono/backend-md  
**Frontend**: https://github.com/Nojorono/frontend-md-dashboard  
**Last Updated**: [Current Date]  
**Version**: 2.1.0



┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │    Backend      │    │  Infrastructure │
│   (Port 9000)   │◄──►│   (Port 9001)   │◄──►│   Services      │
│   Vue.js SPA    │    │   NestJS API    │    │                 │
└─────────────────┘    └─────────────────┘    │ • PostgreSQL    │
                                              │ • RabbitMQ      │
                                              │ • Redis         │
                                              └─────────────────┘
