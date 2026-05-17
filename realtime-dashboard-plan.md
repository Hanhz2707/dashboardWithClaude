# Real-Time Dashboard Microservice Project Plan

## Stack
| Layer | Tech |
|-------|------|
| Frontend | React |
| Auth | Keycloak (OAuth2 / OpenID Connect) via Docker |
| Gateway | Spring Cloud Gateway |
| Backend | Spring Boot 3.x microservices |
| Real-time | WebSocket + STOMP |
| AI | Anthropic Claude API (summarize dashboard data) |
| Messaging | Kafka (Docker) |
| Cache | Redis (Docker) |
| Database | PostgreSQL (Docker) |
| Build | Maven (monorepo with parent pom) |
| Deploy | AWS (at the end) |

## Services
| Service | Port | Description |
|---------|------|-------------|
| api-gateway | 8080 | Spring Cloud Gateway, routes + Keycloak auth |
| dashboard-service | 8081 | WebSocket + STOMP, mock metrics, AI summary |
| metric-service | 8082 | Generates/stores mock metrics, REST API |
| notification-service | 8083 | Listens to Kafka, pushes alerts via WebSocket |
| react-frontend | 3000 | Dashboard UI, Keycloak login, live charts |

## Infrastructure (Docker)
- Keycloak — auth server
- PostgreSQL — persistent storage
- Kafka — async event messaging
- Redis — caching / sessions

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

## Build Order
1. [ ] Install Docker Desktop
2. [ ] Set up Docker Compose (Keycloak, PostgreSQL, Kafka, Redis)
3. [ ] Create parent Maven project with modules
4. [ ] Build api-gateway
5. [ ] Build metric-service (mock data + REST)
6. [ ] Build dashboard-service (WebSocket + AI summary)
7. [ ] Build notification-service (Kafka consumer)
8. [ ] Build React frontend (login + live charts)
9. [ ] Containerize all Spring Boot services
10. [ ] Deploy to AWS

## Notes
- Keycloak runs in Docker (no standalone install needed)
- AI uses Anthropic Claude API to summarize dashboard metrics
- Mock metrics used (random data) — no real data source needed
- AWS deployment is last step
