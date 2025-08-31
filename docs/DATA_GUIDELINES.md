# Data Layer Guidelines

Patterns for API integration, persistence, and repositories.

## API Integration

- Use Retrofit for networking
- Handle errors gracefully
- Use DTOs for API models

## API Documentation

For external APIs, refer to:

- [Sample OpenAPI Spec](https://swagger.io/specification/)
- Document endpoints in `docs/api/` if custom API is used

_Sample contract:_
```yaml
openapi: "3.0.0"
info:
  title: "Track My Finance API"
  version: "1.0.0"
paths:
  /expenses:
    get:
      summary: "List all expenses"
      responses:
        '200':
          description: "A list of expenses"
```

## Local Persistence

- Room for local DB
- Migrate schemas carefully

## Repository Pattern

- Abstract data sources
- Inject repositories via DI
