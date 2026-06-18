# Patient Service Management

## Prerequisites

- Java 21
- Maven 3.9+
- Docker Desktop

---

## Running Locally (VS Code)

### 1. Start infrastructure

```bash
docker compose up -d
```

This starts:
- `patient-service-db` — PostgreSQL on port 5432
- `auth-service-db` — PostgreSQL on port 5433
- `kafka` — Kafka (KRaft mode) with external listener on port 9094

### 2. Start services

Use the Run & Debug panel (Ctrl+Shift+D) in VS Code to launch each service. Start them in this order:

1. **billing-service** — gRPC server must be up before patient-service connects
2. **auth-service** and **patient-service** — either order
3. **analytics-service**
4. **api-gateway** — last; routes to all the above

All launch configurations are pre-configured in `.vscode/launch.json` with the correct environment variables.

---

## Service Ports

| Service          | Port |
|------------------|------|
| patient-service  | 4000 |
| billing-service  | 4001 (HTTP), 9001 (gRPC) |
| analytics-service| 4002 |
| api-gateway      | 4004 |
| auth-service     | 4005 |

---

## Notes

- The api-gateway uses the `local` Spring profile when launched from VS Code, which routes to `localhost` instead of Docker service names.
- The `docker-compose.yml` covers infrastructure only (databases + Kafka). The Java services run directly from VS Code.
