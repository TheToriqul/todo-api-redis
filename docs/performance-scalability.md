# Performance and Scalability Guide

## Overview
This guide outlines performance optimization strategies and scalability considerations for the Todo API microservices architecture.

## Performance Optimization

### Redis Optimization
1. **Connection Pooling**
```javascript
const Redis = require('ioredis');
const pool = new Redis.Cluster([
  { host: 'redis-1', port: 6379 },
  { host: 'redis-2', port: 6379 }
]);
```

2. **Caching Strategies**
- Implement Redis caching for frequently accessed tasks
- Use appropriate TTL (Time To Live) values
```javascript
// Example cache implementation
async function getTaskWithCache(taskId) {
  const cachedTask = await redis.get(`task:${taskId}`);
  if (cachedTask) return JSON.parse(cachedTask);
  
  const task = await fetchTaskFromDB(taskId);
  await redis.setex(`task:${taskId}`, 3600, JSON.stringify(task));
  return task;
}
```

### API Gateway Optimization
1. **Request Rate Limiting**
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use(limiter);
```

2. **Response Compression**
```javascript
const compression = require('compression');
app.use(compression());
```

## Scalability Strategies

### Horizontal Scaling
1. **Service Replication**
```yaml
# docker-compose.yml
services:
  todo-service:
    deploy:
      replicas: 3
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
```

2. **Load Balancing**
```javascript
// nginx.conf example
upstream todo_services {
    least_conn;
    server todo-service-1:8000;
    server todo-service-2:8000;
    server todo-service-3:8000;
}
```

### Database Scaling
1. **Redis Cluster Configuration**
```bash
redis-cli --cluster create \
  127.0.0.1:7000 127.0.0.1:7001 \
  127.0.0.1:7002 127.0.0.1:7003 \
  127.0.0.1:7004 127.0.0.1:7005 \
  --cluster-replicas 1
```

2. **Sharding Strategy**
```javascript
const cluster = new Redis.Cluster([
  { host: 'redis-1', port: 6379 },
  { host: 'redis-2', port: 6379 }
], {
  scaleReads: 'slave',
  natMap: {...}
});
```

## Monitoring and Metrics

### Prometheus Integration
```javascript
const prometheus = require('prom-client');
const collectDefaultMetrics = prometheus.collectDefaultMetrics;

// Create custom metrics
const httpRequestDurationMicroseconds = new prometheus.Histogram({
  name: 'http_request_duration_ms',
  help: 'Duration of HTTP requests in ms',
  labelNames: ['route']
});
```

### Health Checks
```javascript
app.get('/health', async (req, res) => {
  try {
    await redis.ping();
    res.status(200).json({ status: 'healthy' });
  } catch (error) {
    res.status(500).json({ status: 'unhealthy', error: error.message });
  }
});
```

## Performance Testing

### Load Testing with Artillery
```yaml
# artillery-config.yml
config:
  target: "http://localhost:5000"
  phases:
    - duration: 60
      arrivalRate: 5
      rampTo: 50
scenarios:
  - flow:
      - post:
          url: "/tasks"
          json:
            task: "Load test task"
```

### Benchmarking
```bash
# Run benchmark tests
npm run benchmark

# Sample benchmark output
├── Create Task
│   └── 1000 requests: Avg 45ms, Max 120ms
├── Get Tasks
│   └── 1000 requests: Avg 30ms, Max 80ms
└── Delete Task
    └── 1000 requests: Avg 35ms, Max 90ms
```

## Best Practices
1. Always test performance under realistic load conditions
2. Monitor resource usage and set appropriate alerts
3. Implement circuit breakers for resilience
4. Use connection pooling for database connections
5. Implement caching strategies based on access patterns