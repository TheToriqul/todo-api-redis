# Deployment Guide

## Overview
This guide provides instructions for deploying the Todo API in various environments.

## Prerequisites
- Docker
- Docker Compose
- Node.js v14 or higher (for local development)
- Access to a Redis instance (for production)

## Deployment Options

### Local Development
1. Clone the repository:
```bash
git clone https://github.com/TheToriqul/todo-api-redis.git
cd todo-api-redis
```

2. Start the services:
```bash
docker-compose up --build
```

### Production Deployment

#### 1. Environment Setup
Create a `.env.prod` file:
```plaintext
NODE_ENV=production
PORT=5000
REDIS_URL=your-redis-url
```

#### 2. Docker Compose Production Configuration
Create `docker-compose.prod.yml`:
```yaml
version: '3.8'

services:
  redis:
    image: 'redis/redis-stack:latest'
    volumes:
      - redis-data:/data
    restart: always

  api-gateway:
    build: 
      context: ./api-gateway
      dockerfile: Dockerfile.prod
    ports:
      - '5000:5000'
    environment:
      - NODE_ENV=production
      - TODO_SERVICE_URL=http://todo-service:8000
    restart: always
    depends_on:
      - todo-service

  todo-service:
    build:
      context: ./todo-service
      dockerfile: Dockerfile.prod
    environment:
      - NODE_ENV=production
      - REDIS_URL=redis://redis:6379
    restart: always
    depends_on:
      - redis

volumes:
  redis-data:
```

#### 3. Deploy to Production
```bash
docker-compose -f docker-compose.prod.yml up -d
```

### Cloud Deployment

#### AWS ECS
1. Create ECS cluster
2. Configure task definitions
3. Set up service discovery
4. Deploy services using AWS CLI:
```bash
aws ecs create-service \
  --cluster todo-api-cluster \
  --service-name todo-api \
  --task-definition todo-api:1 \
  --desired-count 2
```

#### Google Cloud Run
1. Build and push container images
2. Deploy services:
```bash
gcloud run deploy todo-api \
  --image gcr.io/project-id/todo-api \
  --platform managed
```

## Monitoring and Maintenance

### Health Checks
Implement health check endpoints:
```javascript
app.get('/health', (req, res) => {
  res.status(200).json({ status: 'healthy' });
});
```

### Logging
Use structured logging:
```javascript
const winston = require('winston');
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});
```

### Backup and Recovery
1. Schedule regular Redis backups
2. Implement backup rotation
3. Test recovery procedures

## Security Considerations
1. Use HTTPS in production
2. Implement rate limiting
3. Set up proper firewall rules
4. Use secrets management
5. Regular security updates

## Scaling
1. Horizontal scaling via Docker Compose:
```bash
docker-compose -f docker-compose.prod.yml up -d --scale todo-service=3
```

2. Load balancer configuration
3. Redis clustering for high availability

## Troubleshooting
1. Check container logs:
```bash
docker-compose logs [service-name]
```

2. Monitor resource usage:
```bash
docker stats
```

3. Verify network connectivity:
```bash
docker network inspect todo-api_default
```