# Testing Guide

## Overview
This document outlines the testing strategy and procedures for the Todo API project.

## Test Structure
The project uses Jest as the testing framework and includes the following types of tests:

1. Unit Tests
2. Integration Tests
3. End-to-End Tests

## Running Tests

### Prerequisites
- Node.js v14 or higher
- Docker and Docker Compose

### Setup
1. Install dependencies:
```bash
npm install
```

2. Start the test environment:
```bash
docker-compose -f docker-compose.test.yml up -d
```

### Running Tests
```bash
# Run all tests
npm test

# Run unit tests only
npm run test:unit

# Run integration tests only
npm run test:integration

# Run e2e tests only
npm run test:e2e

# Run tests with coverage
npm run test:coverage
```

## Test Categories

### Unit Tests
Located in `__tests__/unit/`
- API Gateway route handlers
- Todo Service controllers
- Utility functions

### Integration Tests
Located in `__tests__/integration/`
- API Gateway to Todo Service communication
- Todo Service to Redis communication
- Error handling and edge cases

### End-to-End Tests
Located in `__tests__/e2e/`
- Complete task lifecycle
- API response validation
- Error scenarios

## Writing Tests

### Unit Test Example
```javascript
describe('TodoController', () => {
  describe('addTask', () => {
    it('should create a new task', async () => {
      const mockTask = { task: 'Test task' };
      const response = await request(app)
        .post('/tasks')
        .send(mockTask);
      
      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
      expect(response.body.task).toBe(mockTask.task);
    });
  });
});
```

### Sample Task Creation Tests
```javascript
describe('Task Creation', () => {
  describe('POST /tasks', () => {
    it('should create Sample Task 1', async () => {
      const response = await request(app)
        .post('/')
        .set('Content-Type', 'application/json')
        .send({ task: 'Sample Task 1' });
      
      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
      expect(response.body.task).toBe('Sample Task 1');
      
      // Store task ID for later tests
      sampleTask1Id = response.body.id;
    });

    it('should create Sample Task 2', async () => {
      const response = await request(app)
        .post('/')
        .set('Content-Type', 'application/json')
        .send({ task: 'Sample Task 2' });
      
      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
      expect(response.body.task).toBe('Sample Task 2');
      
      // Store task ID for later tests
      sampleTask2Id = response.body.id;
    });

    it('should retrieve both created tasks', async () => {
      const response = await request(app)
        .get('/');
      
      expect(response.status).toBe(200);
      expect(response.body).toBeInstanceOf(Array);
      expect(response.body.length).toBe(2);
      
      // Verify both tasks exist in the response
      const tasks = response.body.map(task => task.task);
      expect(tasks).toContain('Sample Task 1');
      expect(tasks).toContain('Sample Task 2');
    });
  });
});

// Curl command equivalent tests using axios
describe('Curl Command Tests', () => {
  it('should match curl command for Sample Task 1', async () => {
    const response = await axios.post('http://localhost:5000/', {
      task: 'Sample Task 1'
    }, {
      headers: {
        'Content-Type': 'application/json'
      }
    });
    
    expect(response.status).toBe(201);
    expect(response.data.task).toBe('Sample Task 1');
  });

  it('should match curl command for Sample Task 2', async () => {
    const response = await axios.post('http://localhost:5000/', {
      task: 'Sample Task 2'
    }, {
      headers: {
        'Content-Type': 'application/json'
      }
    });
    
    expect(response.status).toBe(201);
    expect(response.data.task).toBe('Sample Task 2');
  });
});
```

## Code Coverage
The project aims to maintain a test coverage of at least 80%. Coverage reports are generated using Jest and can be found in the `coverage/` directory after running:
```bash
npm run test:coverage
```

## Continuous Integration
Tests are automatically run on GitHub Actions for:
- Pull requests to main branch
- Direct pushes to main branch
- Nightly builds

## Running Sample Tasks Manually
To test the API manually using curl commands:

```bash
# Create Sample Task 1
curl -X POST http://localhost:5000/ -H "Content-Type: application/json" -d '{"task": "Sample Task 1"}'

# Create Sample Task 2
curl -X POST http://localhost:5000/ -H "Content-Type: application/json" -d '{"task": "Sample Task 2"}'

# Verify tasks were created
curl http://localhost:5000/
```

## Best Practices
1. Follow the AAA (Arrange-Act-Assert) pattern
2. Use meaningful test descriptions
3. Mock external dependencies
4. Test edge cases and error conditions
5. Keep tests focused and atomic
6. Include both automated tests and manual test procedures
7. Document test data and expected results
8. Maintain test isolation and cleanup