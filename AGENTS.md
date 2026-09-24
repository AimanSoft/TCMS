# AGENTS.md - OpenCode Configuration

## Project Information

- **Project Name**: TCMS (Travel Cost Management System)
- **Description**: A microservices-based travel cost management system with API Gateway and Event-Driven Architecture
- **Created**: 2026-09-24

## Technology Stack

- **Language**: TypeScript
- **Framework**: NestJS
- **Architecture**: Microservices + API Gateway + Event-Driven
- **Message Broker**: RabbitMQ
- **Databases**: PostgreSQL + MongoDB
- **Cache**: Redis
- **Containerization**: Docker + Kubernetes
- **Frontend**: React/Angular + Ant Design
- **Testing**: Jest, Cypress, Supertest

## Project Structure

```
TCMS/
├── docs/                    # Documentation
│   ├── analysis.md         # Preliminary analysis
│   ├── architecture.md     # Architecture documentation
│   ├── ui-ux-spec.md       # UI/UX specifications
│   ├── data-flow.md        # Data flow diagrams
│   ├── flow-of-events.md   # Event flow documentation
│   ├── use-cases.md        # Use cases documentation
│   ├── mindmap.md          # Project mind map
│   ├── implement-plan.md   # Implementation plan
│   └── phases/            # Phase-specific todos
│       ├── phase-1-todo.md
│       └── phase-2-todo.md
├── src/                     # Source code
│   ├── api-gateway/       # API Gateway service
│   ├── user-service/      # User microservice
│   ├── request-service/   # Request microservice
│   ├── approval-service/  # Approval microservice
│   ├── payment-service/   # Payment microservice
│   ├── report-service/    # Report microservice
│   └── shared/            # Shared modules and utilities
├── docker/                # Docker configurations
├── k8s/                   # Kubernetes configurations
├── tests/                 # Test files
├── package.json
├── tsconfig.json
├── nest-cli.json
├── docker-compose.yml
└── AGENTS.md
```

## Coding Standards

### TypeScript
- Use strict mode
- Follow ESLint rules
- Use interfaces for type definitions
- Use async/await for asynchronous operations
- Follow SOLID principles

### NestJS
- Use modules, controllers, and providers
- Follow dependency injection pattern
- Use guards and interceptors
- Implement pipes for validation
- Use DTOs for request/response modeling

### Documentation
- Document all public APIs
- Write clear commit messages
- Update AGENTS.md after major changes
- Keep docs in sync with code

## Architecture Patterns

- **CQRS**: Command Query Responsibility Segregation
- **Saga Pattern**: Distributed transaction management
- **Event Sourcing**: State tracking through events
- **Circuit Breaker**: Fault tolerance
- **API Gateway**: Centralized entry point

## Event-Driven Architecture

### RabbitMQ Configuration
- Exchange types: direct, topic, fanout
- Dead Letter Queue for failed messages
- Retry policies for message processing
- Message serialization: JSON

### Event Types
- `PaymentCompletedEvent`
- `RequestSubmittedEvent`
- `ApprovalGrantedEvent`
- `RequestRejectedEvent`
- `NotificationSentEvent`

## Development Commands

```bash
# Start development
npm run start:dev

# Build
npm run build

# Test
npm run test

# Lint
npm run lint

# Docker
docker-compose up -d
docker-compose down

# Run tests
npm run test:unit
npm run test:integration
npm run test:e2e
```

## Team

- **Project Lead**: System Architect
- **Backend Team**: 4-6 developers
- **Frontend Team**: 2-3 developers
- **QA Team**: 2-3 testers
- **DevOps**: 1 engineer

## Notes

- Always use TypeScript strictly typed
- Follow NestJS module structure conventions
- Document all events and their schemas
- Maintain backward compatibility for APIs
- Use environment variables for configuration
- Implement proper error handling and logging
