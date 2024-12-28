# API Documentation

## Overview
This document describes the RESTful API endpoints provided by the Todo API service.

## Base URL
- Development: `http://localhost:5000`
- Production: Configure based on your deployment environment

## Endpoints

### Get All Tasks
- **Method**: GET
- **Endpoint**: `/`
- **Description**: Retrieves all tasks from the system
- **Response**: Array of task objects
```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "task": "Sample Task 1"
  },
  {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "task": "Sample Task 2"
  }
]
```

### Get Task by ID
- **Method**: GET
- **Endpoint**: `/:id`
- **Parameters**: 
  - `id` (path parameter): UUID of the task
- **Response**: Single task object
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "task": "Sample Task 1"
}
```
- **Error Response**: 404 if task not found

### Create Task
- **Method**: POST
- **Endpoint**: `/`
- **Request Body**:
```json
{
  "task": "New task description"
}
```
- **Response**: Created task object with ID
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "task": "New task description"
}
```

### Delete Task
- **Method**: DELETE
- **Endpoint**: `/:id`
- **Parameters**:
  - `id` (path parameter): UUID of the task to delete
- **Response**: Success message
```json
{
  "message": "Task deleted"
}
```
- **Error Response**: 404 if task not found

## Error Handling
The API uses standard HTTP response codes:
- 200: Success
- 201: Created
- 400: Bad Request
- 404: Not Found
- 500: Internal Server Error

Error responses include a message describing the error:
```json
{
  "error": "Task not found"
}
```