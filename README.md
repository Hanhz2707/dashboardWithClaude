# Real-Time Dashboard Microservice

A full-stack real-time dashboard application built with Spring Boot microservices, Keycloak authentication, WebSocket, Kafka, and React.

## Architecture

```
React ──▶ API Gateway ──▶ Keycloak (Docker)
                │
     ┌──────────┼──────────┐
     ▼          ▼          ▼
 Dashboard   Metric    Notification
 Service     Service    Service
     │          │          │
     └──────────┴──▶ Kafka (Docker)
                │
           PostgreSQL (Docker)
           Redis (Docker)
```

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React |
| Auth | Keycloak (OAuth2 / OpenID Connect) |
| Gateway | Spring Cloud Gateway |
| Backend | Spring Boot 3.3.5 |
| Real-time | WebSocket + STOMP |
| AI | Anthropic Claude API |
| Messaging | Apache Kafka |
| Cache | Redis |
| Database | PostgreSQL |
| Build | Maven (monorepo) |
| Deploy | AWS |

## Services

| Service | Port | Description |
|---------|------|-------------|
| api-gateway | 8080 | Single entry point, routes requests, validates Keycloak tokens |
| metric-service | 8082 | Generates mock metrics, stores in PostgreSQL, publishes to Kafka |
| dashboard-service | 8081 | WebSocket endpoint, broadcasts live data, Claude AI summaries |
| notification-service | 8083 | Consumes Kafka events, pushes real-time alerts |
| react-frontend | 3000 | Dashboard UI with live charts and Keycloak login |

## Infrastructure (Docker)

| Container | Port | Purpose |
|-----------|------|---------|
| dashboard-postgres | 5432 | Database for app data and Keycloak |
| dashboard-keycloak | 8180 | Auth server |
| dashboard-zookeeper | 2181 | Kafka cluster coordinator |
| dashboard-kafka | 9092 | Message broker |
| dashboard-redis | 6379 | Cache and session store |

## Prerequisites

- Java 21
- Maven 3.9+
- Docker Desktop
- Node.js 18+ (for React frontend)

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Hanhz2707/dashboardWithClaude.git
cd dashboardWithClaude
```

### 2. Start infrastructure

```bash
docker compose up -d
```

Wait for all containers to be healthy:

```bash
docker compose ps
```

### 3. Configure Keycloak

1. Open `http://localhost:8180`
2. Login with `admin / admin123`
3. Create a new realm: `dashboard`
4. Create a client: `dashboard-client`
5. Create roles: `ROLE_USER`, `ROLE_ADMIN`
6. Create a test user and assign roles

### 4. Build all services

```bash
mvn clean install
```

### 5. Run each service

Open a separate terminal for each:

```bash
# API Gateway
cd api-gateway && mvn spring-boot:run

# Metric Service
cd metric-service && mvn spring-boot:run

# Dashboard Service
cd dashboard-service && mvn spring-boot:run

# Notification Service
cd notification-service && mvn spring-boot:run
```

### 6. Run the frontend

```bash
cd react-frontend
npm install
npm start
```

Open `http://localhost:3000` in your browser.

## Project Structure

```
dashboardWithClaude/
├── docker-compose.yml
├── pom.xml                          # Parent Maven POM
├── api-gateway/
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/dashboard/gateway/
│       └── resources/application.yml
├── metric-service/
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/dashboard/metric/
│       └── resources/application.yml
├── dashboard-service/
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/dashboard/dashboard/
│       └── resources/application.yml
├── notification-service/
│   ├── pom.xml
│   └── src/main/
│       ├── java/com/dashboard/notification/
│       └── resources/application.yml
└── react-frontend/
```

## Environment Variables

Create a `.env` file in the root or set these in each service's `application.yml`:

```env
# PostgreSQL
DB_HOST=localhost
DB_PORT=5432
DB_NAME=dashboarddb
DB_USER=dashboard
DB_PASSWORD=dashboard123

# Keycloak
KEYCLOAK_URL=http://localhost:8180
KEYCLOAK_REALM=dashboard
KEYCLOAK_CLIENT_ID=dashboard-client

# Kafka
KAFKA_BOOTSTRAP_SERVERS=localhost:9092

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Anthropic Claude AI
ANTHROPIC_API_KEY=your_api_key_here
```

## License

MIT
