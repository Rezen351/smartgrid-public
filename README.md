# SmartGrid Public

A sanitized reference architecture for an IoT smart-energy platform. The project demonstrates how containerized services can combine device messaging, authentication, time-series data, monitoring, and machine-learning-assisted energy decisions.

> **Status:** Public reference edition. The production SmartGrid repository remains private.

## Highlights

- Docker Compose-based local infrastructure
- MQTT and NATS messaging patterns
- PostgreSQL and time-series storage
- Prometheus and Grafana observability
- Modular Go services for authentication, device logic, and data export
- Python/FastAPI machine-learning service with safety guardrails
- Environment-based configuration with no credentials committed

## Architecture

```text
IoT devices
    │
    ├── MQTT / NATS
    │
    ▼
┌───────────────┐     ┌──────────────────┐
│ Go services   │────▶│ PostgreSQL / TSDB │
│ auth/module   │     └──────────────────┘
│ export        │
└───────┬───────┘
        │
        ▼
┌───────────────────────┐
│ Dashboard & ML service│
└───────────┬───────────┘
            │
            ▼
     Prometheus / Grafana
```

## Quick start

### Requirements

- Docker Engine
- Docker Compose v2
- Git

### Run locally

```bash
git clone https://github.com/Rezen351/smartgrid-public.git
cd smartgrid-public
cp .env.example .env
# Replace every CHANGE_ME_* value before starting services.
docker compose up -d
```

Validate the configuration and inspect the services:

```bash
docker compose config
docker compose ps
docker compose logs -f
```

The ML service, when included in this public edition, can be started with:

```bash
docker compose -f docker-compose.ml-host.yml up -d
```

## Repository layout

```text
infra/                 Infrastructure configuration
services/auth/         Authentication and RBAC service
services/module/       IoT module logic
services/export/       Time-series export service
services/dashboard/   HMI/API dashboard
services/ml/           ML inference service
tests/                 Automated tests
docs/                  Architecture and operational notes
```

## Security

- Never commit `.env`, tokens, private keys, passwords, or production URLs.
- Use `.env.example` as the only committed configuration template.
- Rotate any credential that may have been exposed in Git history.
- Review Git history separately when auditing leaks; deleting a secret from the latest commit does not remove it from past commits.
- Do not expose database, broker, or monitoring ports to the public internet without authentication and network controls.
- Treat this repository as a learning/reference deployment, not a production-ready turnkey system.

## Testing

```bash
for service in auth module export; do
  (cd "services/$service" && go test ./... && go vet ./...)
done

(cd services/dashboard && python -m py_compile app.py history.py research.py)
```

## Contributing

Issues and pull requests are welcome for documentation, tests, security improvements, and deployment portability. Please avoid submitting real credentials, private infrastructure details, or customer data.

## License

MIT License. See [LICENSE](LICENSE).
