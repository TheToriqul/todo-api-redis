# Contributing Guide

## Welcome
Thank you for considering contributing to the Todo API project! This document provides guidelines and instructions for contributing.

## Code of Conduct
Please note that this project is released with a Contributor Code of Conduct. By participating in this project you agree to abide by its terms.

## Getting Started

### Prerequisites
- Node.js (v14 or higher)
- Docker and Docker Compose
- Git

### Setting Up Development Environment
1. Fork the repository
2. Clone your fork:
```bash
git clone https://github.com/YOUR-USERNAME/todo-api-redis.git
cd todo-api-redis
```

3. Add upstream remote:
```bash
git remote add upstream https://github.com/TheToriqul/todo-api-redis.git
```

4. Install dependencies:
```bash
npm install
```

## Development Workflow

### 1. Create a Branch
```bash
# Create and switch to a new branch
git checkout -b feature/your-feature-name
```

### 2. Make Changes
- Write your code
- Follow the coding standards
- Add tests for new features
- Update documentation as needed

### 3. Commit Changes
```bash
# Stage changes
git add .

# Commit with a descriptive message
git commit -m "feat: add new feature X"
```

#### Commit Message Guidelines
We follow the Conventional Commits specification:
- feat: New feature
- fix: Bug fix
- docs: Documentation changes
- style: Code style changes
- refactor: Code refactoring
- test: Test changes
- chore: Build process or auxiliary tool changes

### 4. Keep Your Branch Updated
```bash
git fetch upstream
git rebase upstream/main
```

### 5. Push Changes
```bash
git push origin feature/your-feature-name
```

### 6. Create Pull Request
- Go to GitHub and create a Pull Request
- Fill in the PR template
- Reference any related issues

## Coding Standards

### JavaScript Style Guide
We follow the Airbnb JavaScript Style Guide:
```javascript
// Good
const foo = {
  bar: 'baz',
  qux: 'quux'
};

// Bad
const foo = {
  bar:'baz',
  qux:'quux'
}
```

### Tests
- Write tests for new features
- Maintain or improve code coverage
```javascript
describe('Feature', () => {
  it('should do something', () => {
    // Arrange
    const input = 'test';
    
    // Act
    const result = doSomething(input);
    
    // Assert
    expect(result).toBe('expected');
  });
});
```

## Review Process

### Pull Request Reviews
1. Code quality check
2. Test coverage verification
3. Documentation review
4. Performance impact assessment
5. Security consideration review

### Merge Requirements
- Passing CI/CD pipeline
- Approved code review
- No merge conflicts
- Meets coding standards
- Updated documentation

## Release Process

### Version Control
We use Semantic Versioning:
- MAJOR version for incompatible API changes
- MINOR version for backward-compatible functionality
- PATCH version for backward-compatible bug fixes

### Release Steps
1. Update version in package.json
2. Update CHANGELOG.md
3. Create release tag
4. Deploy to staging
5. Verify deployment
6. Deploy to production

## Getting Help
- Create an issue for bugs
- Join our Discord community
- Check the documentation
- Contact maintainers

## Recognition
Contributors will be added to CONTRIBUTORS.md file.

Thank you for contributing to Todo API! 🎉