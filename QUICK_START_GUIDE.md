# Quick Start Guide - Docker Setup

## 🚀 Quick Deployment

### Prerequisites
- Docker and Docker Compose installed
- Git repository cloned

### 1. Build and Run Everything
```bash
# Build and start both services
docker-compose up --build

# Run in background
docker-compose up --build -d
```

### 2. Access Applications
- **Frontend**: http://localhost:9000
- **Backend**: http://localhost:9001
- **Backend Health**: http://localhost:9001/health
- **RabbitMQ Management**: http://localhost:15672 (admin/master123)
- **PostgreSQL**: localhost:5432 (admin/master123)
- **Redis**: localhost:6379 (password: master123)

### 3. View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f backend
docker-compose logs -f frontend
```

### 4. Stop Services
```bash
# Stop and remove containers
docker-compose down

# Stop and remove containers + volumes
docker-compose down -v
```

---

## 🔧 Individual Service Management

### Backend Only
```bash
# Build backend
cd backend
docker build -t backend-app .

# Run backend
docker run -p 9001:9001 backend-app
```

### Frontend Only
```bash
# Build frontend
cd frontend
docker build -t frontend-app .

# Run frontend
docker run -p 9000:9000 frontend-app
```

---

## 📊 Health Monitoring

### Check Container Status
```bash
# View running containers
docker ps

# View container health
docker inspect --format='{{.State.Health.Status}}' md-backend
docker inspect --format='{{.State.Health.Status}}' md-frontend
```

### Health Check Endpoints
- Backend: `GET http://localhost:9001/health`
- Frontend: `GET http://localhost:9000/`
- Database: `docker exec md-postgres pg_isready`
- Redis: `docker exec md-redis redis-cli ping`
- RabbitMQ: `docker exec md-rabbitmq rabbitmq-diagnostics ping`

---

## 🐛 Troubleshooting

### Common Issues

#### 1. Port Already in Use
```bash
# Check what's using the port
netstat -tulpn | grep :9000
netstat -tulpn | grep :9001

# Kill process using port
sudo kill -9 <PID>
```

#### 2. Build Failures
```bash
# Clean Docker cache
docker system prune -a

# Rebuild without cache
docker-compose build --no-cache
```

#### 3. Container Won't Start
```bash
# Check container logs
docker-compose logs backend
docker-compose logs frontend

# Check container status
docker-compose ps
```

#### 4. Permission Issues
```bash
# Fix file permissions (Linux/Mac)
sudo chown -R $USER:$USER .

# On Windows, run Docker Desktop as Administrator
```

---

## 🔍 Debugging Commands

### Container Inspection
```bash
# Enter running container
docker exec -it md-backend sh
docker exec -it md-frontend sh

# View container details
docker inspect md-backend
docker inspect md-frontend
```

### Resource Usage
```bash
# View resource usage
docker stats

# View disk usage
docker system df
```

---

## 📝 Environment Variables

### Backend Environment
1. Copy the example file: `cp env.example .env.docker`
2. Edit `.env.docker` with your actual values:
```env
NODE_ENV=production
DATABASE_URL=postgresql://admin:master123@postgres:5432/postgres
RABBITMQ_URL=amqp://admin:master123@rabbitmq:5672
REDIS_URL=redis://:master123@redis:6379
JWT_SECRET=your_jwt_secret
AWS_ACCESS_KEY_ID=your_actual_aws_key
AWS_SECRET_ACCESS_KEY=your_actual_aws_secret
```

### Frontend Environment
Create `.env.docker` file in frontend directory:
```env
VUE_APP_API_URL=http://localhost:9001
VUE_APP_PORT=9000
```

---

## 🔄 Development Workflow

### 1. Development Mode
```bash
# Start services in development mode
docker-compose -f docker-compose.dev.yml up
```

### 2. Production Mode
```bash
# Start services in production mode
docker-compose up --build
```

### 3. Hot Reload (Development)
```bash
# Mount source code for hot reload
docker-compose -f docker-compose.dev.yml up --build
```

---

## 📋 Useful Commands Cheat Sheet

```bash
# Build and run
docker-compose up --build

# Run in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild specific service
docker-compose build backend

# Scale services
docker-compose up --scale backend=3

# View resource usage
docker stats

# Clean up
docker system prune -a

# Check health
curl http://localhost:9001/health
curl http://localhost:9000/
```

---

## 🆘 Need Help?

### Check Documentation
- Full documentation: `DOCKER_SETUP_DOCUMENTATION.md`
- Docker Compose reference: https://docs.docker.com/compose/

### Common Solutions
1. **Service won't start**: Check logs with `docker-compose logs`
2. **Build fails**: Clean cache with `docker system prune -a`
3. **Port conflicts**: Check with `netstat -tulpn | grep :9000`
4. **Permission issues**: Run Docker Desktop as Administrator (Windows)

---

**Last Updated**: [Current Date]  
**Version**: 1.0 