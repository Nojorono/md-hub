A production-ready application with Docker containerization, comprehensive security measures, and optimized performance.

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose
- Node.js 20+ (for local development)
- Yarn package manager

### Environment Setup

1. **Copy environment template:**
```bash
cp env.example .env.docker
```

2. **Configure your environment variables:**
```bash
# Edit .env.docker with your actual values
nano .env.docker
```

### Docker Deployment (Recommended)

#### Start Complete Infrastructure Stack
```bash
# From project root (md-b/)
docker-compose up --build
```

This starts:
- **Backend API**: http://localhost:9001
- **Frontend**: http://localhost:9000
- **PostgreSQL**: localhost:5432 (admin/master123)
- **RabbitMQ**: localhost:5672, Management UI: http://localhost:15672
- **Redis**: localhost:6379 (password: master123)

### Local Development

#### Start Core Services
```bash
# Start database services
yarn dep:up
```

#### Run Backend in Development Mode
```bash
# Install dependencies
yarn install

# Start development server
yarn dev
```

#### Stop Core Services
```bash
yarn dep:down
```

## 📋 API Documentation

- **Health Check**: `GET http://localhost:9001/health`
- **Swagger Docs**: `GET http://localhost:9001/api/docs`
- **API Base URL**: `http://localhost:9001/api`

## 🔧 Configuration

### Environment Variables

Create `.env.docker` file with the following variables:

```env
# Database Configuration
DATABASE_URL=postgresql://admin:master123@postgres:5432/postgres

# Message Queue Configuration
RABBITMQ_URL=amqp://admin:master123@rabbitmq:5672

# Cache Configuration
REDIS_URL=redis://:master123@redis:6379

# JWT Configuration
JWT_SECRET=your_jwt_secret_here
JWT_REFRESH_SECRET=your_jwt_refresh_secret_here

# AWS Configuration
AWS_ACCESS_KEY_ID=your_aws_access_key_id_here
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key_here
AWS_REGION=your_aws_region_here
AWS_S3_BUCKET=your_s3_bucket_name_here

# Application Configuration
NODE_ENV=production
PORT=9001

# Email Configuration
SMTP_HOST=your_smtp_host_here
SMTP_PORT=587
SMTP_USER=your_smtp_user_here
SMTP_PASS=your_smtp_password_here

# Other Configuration
API_PREFIX=/api
CORS_ORIGIN=http://localhost:9000
```

### Environment Files
- **`.env.local`**: Local development environment
- **`.env.docker`**: Docker compose environment
- **`.env`**: Production environment
- **`env.example`**: Template file (safe to commit)

## 🐳 Docker Configuration

### Multi-Stage Build
The Dockerfile uses a multi-stage build process:
1. **Base**: Common setup for all stages
2. **Deps**: Dependency installation
3. **Builder**: Application compilation
4. **Production**: Final lightweight image

### Security Features
- ✅ Non-root user execution (`nestjs` user)
- ✅ Alpine Linux base image
- ✅ Proper file permissions
- ✅ Signal handling with `dumb-init`
- ✅ Health checks

### Performance Optimizations
- ✅ Layer caching for faster builds
- ✅ Production-only dependencies
- ✅ Frozen lockfile for reproducible builds
- ✅ Cache cleaning

## 📊 Monitoring & Health Checks

### Health Endpoints
- **Application Health**: `GET /health`
- **Database**: `docker exec md-postgres pg_isready`
- **Redis**: `docker exec md-redis redis-cli ping`
- **RabbitMQ**: `docker exec md-rabbitmq rabbitmq-diagnostics ping`

### Logging
- Structured JSON logging
- Error tracking and reporting
- Performance monitoring

## 🔒 Security Considerations

### Implemented Security Measures
- Non-root container execution
- Minimal attack surface (Alpine Linux)
- Proper file ownership and permissions
- Graceful signal handling
- Health monitoring

### Best Practices
- Never commit `.env.docker` files to Git
- Use environment variables for secrets
- Regular security updates
- Image vulnerability scanning

## 🚀 Production Deployment

### Docker Compose (Recommended)
```bash
# Production deployment
docker-compose up --build -d

# View logs
docker-compose logs -f backend

# Scale services
docker-compose up --scale backend=3
```

### Kubernetes Ready
The application is ready for Kubernetes deployment with:
- Health checks
- Proper signal handling
- Non-root execution
- Resource limits support

## 🛠️ Development Commands

```bash
# Install dependencies
yarn install

# Development server
yarn dev

# Build application
yarn build

# Run tests
yarn test
yarn test:e2e

# Linting
yarn lint
yarn lint:fix

# Database migrations
yarn migrate
yarn migrate:prod

# Seed database
yarn seed
```

## 🔗 Related Services

- **Frontend**: Vue.js application (port 9000)
- **Backend**: Nest.Js application (port 9001)
- **Database**: PostgreSQL (port 5432)
- **Message Queue**: RabbitMQ (ports 5672, 15672)
- **Cache**: Redis (port 6379)

## 📚 Additional Documentation

- **Docker Setup**: See `DOCKER_SETUP_DOCUMENTATION.md`
- **Quick Start**: See `QUICK_START_GUIDE.md`
- **API Documentation**: Available at `/api/docs` when running

## 🤝 Contributing

1. Follow the existing code style
2. Add tests for new features
3. Update documentation as needed
4. Ensure all tests pass before submitting

## 📄 License

This project is proprietary and confidential.

---

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
