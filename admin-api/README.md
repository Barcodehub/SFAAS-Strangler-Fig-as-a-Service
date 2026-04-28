# Admin API Module

Spring MVC-based REST API for managing routes and viewing statistics.

## Responsibilities

- CRUD operations for routes
- Route validation
- Optimistic locking
- Gateway cache invalidation
- Statistics aggregation
- Database migrations (Flyway)

## Architecture

### Blocking Flow

```
Request → Controller → Service → JPA Repository → PostgreSQL
    ↓
RestClient → Gateway cache invalidation
```

### Key Components

#### RouteController
- REST endpoints for CRUD
- Bean validation
- Exception handling

#### RouteService
- Business logic
- Path pattern validation
- Cache invalidation after writes
- Transaction management

#### JpaRouteRepository
- Adapter implementing RouteRepository port
- Optimistic locking with @Version
- CompletableFuture for async interface

#### StatsService
- Aggregates request logs
- Calculates percentiles (p50, p95, p99)
- Error rate computation

## Configuration

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/sfaas
    username: sfaas
    password: sfaas123
  
  jpa:
    hibernate:
      ddl-auto: validate
  
  flyway:
    enabled: true
    locations: classpath:db/migration

gateway:
  url: http://localhost:8080
  api-key: secret-key
```

## Running Locally

```powershell
.\gradlew.bat :admin-api:bootRun
```

## API Endpoints

### Routes

#### Create Route
```http
POST /api/routes
Content-Type: application/json

{
  "name": "users-api",
  "pathPattern": "/api/users/**",
  "legacyUrl": "http://localhost:8082",
  "newServiceUrl": "http://localhost:8083",
  "trafficPercentage": 10,
  "enabled": true,
  "timeoutMs": 5000
}
```

#### Get All Routes
```http
GET /api/routes
```

#### Get Route by ID
```http
GET /api/routes/{id}
```

#### Update Route
```http
PUT /api/routes/{id}
Content-Type: application/json

{
  "name": "users-api",
  "pathPattern": "/api/users/**",
  "legacyUrl": "http://localhost:8082",
  "newServiceUrl": "http://localhost:8083",
  "trafficPercentage": 50,
  "enabled": true,
  "timeoutMs": 5000,
  "version": 0
}
```

#### Delete Route
```http
DELETE /api/routes/{id}
```

### Statistics

#### Get Route Stats
```http
GET /api/stats/routes/{id}
```

Response:
```json
{
  "totalRequests": 1000,
  "errorRate": 2.5,
  "p50": 120,
  "p95": 450,
  "p99": 800
}
```

## Validation Rules

- `name`: required, unique
- `pathPattern`: required, valid Ant pattern
- `legacyUrl`: required, valid HTTP(S) URL
- `newServiceUrl`: required, valid HTTP(S) URL
- `trafficPercentage`: 0-100
- `timeoutMs`: 100-60000

## Error Responses

### 400 Bad Request
```json
{
  "errors": {
    "trafficPercentage": "Traffic percentage must be between 0 and 100"
  }
}
```

### 409 Conflict
```json
{
  "error": "Route was modified by another transaction"
}
```

## Database Migrations

Located in `src/main/resources/db/migration/`

- `V1__initial_schema.sql` - Initial tables and indexes

## Testing

```powershell
# Unit tests
.\gradlew.bat :admin-api:test

# Integration tests with Testcontainers
.\gradlew.bat :admin-api:integrationTest
```

## Optimistic Locking

Routes use `@Version` for optimistic locking:

1. Client reads route with version=0
2. Client updates route
3. JPA executes: `UPDATE routes SET ... WHERE id=? AND version=0`
4. If version changed, throws OptimisticLockException → 409 Conflict
5. Client must re-fetch and retry
