# Todo API Command Reference Guide
### Project content table
- [Section 1: Core Project Workflow](#section-1-core-project-workflow)
- [Section 2: Advanced Operations](#section-2-advanced-operations)
- [Section 3: Production Guide](#section-3-production-guide)

> **Author**: [Md Toriqul Islam](https://linkedin.com/TheToriqul)  
> **Description**: Microservices-based Todo API using Node.js, Express, Redis, and Docker   
> **Learning Focus**: Microservices Architecture, Node.js, Redis, Docker  
> **Note**: This is a reference guide. Do not execute commands directly without understanding their implications.

## Section 1: Core Project Workflow

### Step 1: Clone the Repository
```bash
# Clone the project repository
git clone https://github.com/TheToriqul/todo-api-redis.git

# Navigate to the project directory
cd todo-api-redis

# Verify the project files are present
ls -l
```

### Step 2: Build and Start Docker Containers
```bash
# Build and start the Docker containers
docker-compose up --build

# Verify the containers are running
docker ps
```

### Step 3: Interact with the API

#### Get All Tasks
```bash
# Fetch all tasks
curl http://localhost:5000/

# Verify the response contains an array of tasks
```

#### Get a Specific Task
```bash
# Fetch a specific task by ID
curl http://localhost:5000/<task_id>

# Verify the response contains the task details
```

#### Add a New Task
```bash
# Add a new task
curl -X POST http://localhost:5000/ -H "Content-Type: application/json" -d '{"task": "Sample Task"}'

# Verify the response contains the newly created task
```

#### Delete a Task
```bash
# Delete a task by ID
curl -X DELETE http://localhost:5000/<task_id>

# Verify the task is deleted by fetching all tasks
curl http://localhost:5000/
```

### Final Step: Cleanup
```bash
# Stop and remove the Docker containers
docker-compose down

# Verify the containers are removed
docker ps
```

## Section 2: Advanced Operations

### Debugging

#### Check API Gateway Logs
```bash
# View the API Gateway logs
docker-compose logs api-gateway
```

#### Check Todo Service Logs
```bash
# View the Todo Service logs
docker-compose logs todo-service
```

#### Check Redis Logs
```bash
# View the Redis logs
docker-compose logs redis
```

### Scaling Services

#### Scale API Gateway
```bash
# Scale the API Gateway to 3 replicas
docker-compose up -d --scale api-gateway=3
```

#### Scale Todo Service
```bash
# Scale the Todo Service to 5 replicas
docker-compose up -d --scale todo-service=5
```

## Section 3: Production Guide

### Production Setup

#### Update Environment Variables
```bash
# Update environment variables in docker-compose.yml
vim docker-compose.yml

# Set NODE_ENV to 'production'
# Set REDIS_URL to the production Redis URL
```

#### Deploy to Production
```bash
# Build and start containers in production
docker-compose -f docker-compose.prod.yml up --build -d
```

### Monitoring

#### Monitor Container Resources
```bash
# Monitor CPU, memory, and network usage of containers
docker stats
```

#### Monitor API Gateway
```bash
# Monitor API Gateway logs in real-time
docker-compose logs -f api-gateway
```

#### Monitor Todo Service
```bash
# Monitor Todo Service logs in real-time
docker-compose logs -f todo-service
```

### Backup Operations

#### Backup Redis Data
```bash
# Backup Redis data to a file
docker exec -it redis redis-cli SAVE

# Copy the backup file from the Redis container
docker cp redis:/data/dump.rdb /path/to/backup/dump.rdb
```

#### Restore Redis Data
```bash
# Stop the Redis container
docker-compose stop redis

# Copy the backup file to the Redis container
docker cp /path/to/backup/dump.rdb redis:/data/dump.rdb

# Start the Redis container
docker-compose start redis
```

## Learning Notes

1. Microservices architecture allows for modular and scalable development
2. Redis provides fast and efficient data storage for the Todo Service
3. Docker simplifies the deployment and management of microservices
4. API Gateway acts as a single entry point for client requests
5. Node.js and Express enable rapid development of RESTful APIs

---

> 💡 **Best Practice**: Use meaningful and descriptive names for services, variables, and functions.

> ⚠️ **Warning**: Always secure your APIs and protect sensitive data in production environments.

> 📝 **Note**: Regularly update dependencies and apply security patches to maintain a secure system.