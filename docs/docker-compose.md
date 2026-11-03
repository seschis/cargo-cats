# Docker Compose (lightweight local stack)

For quick local development you can run the vulnerable application without Kubernetes using Docker Compose. This stack runs the eight app services and MySQL behind an nginx front door.

**Not included:** Contrast agents (the agent-operator only works in Kubernetes), ModSecurity WAF, Falco, fluent-bit/OpenSearch, the Simulation Console, ZAP, and the exploit server. Use `make deploy` for the full stack.

## Requirements

- Docker Engine with the Compose plugin (`docker compose version`)
- About 4 GB of free memory (about 5 GB with the AI profile)

## Usage

```bash
make compose-up      # build images and start the stack
make compose-down    # stop it and remove containers
```

Or call Compose directly:

```bash
docker compose up -d --build
docker compose ps
docker compose logs -f frontgateservice
docker compose down
```

Open http://app.localhost (or http://localhost) and log in with `admin` / `password123`.

Set `CARGOCATS_HTTP_PORT` to serve on a port other than 80 (e.g. `CARGOCATS_HTTP_PORT=8080 make compose-up`).

## AI features (optional)

Ollama is off by default, and the aiservice chatbot and reportservice shadow AI start disabled. To turn them on:

```bash
AI_ENABLED=true docker compose --profile ai up -d --build
```

The first start pulls `smollm2:135m` (override it with `OLLAMA_MODEL`).

## Notes

- Service hostnames match the Kubernetes service names (`dataservice`, `cargocats-db`, ...), so no service code changes are needed.
- If Maven Central rate-limits your builds (HTTP 429), build the Java services one at a time: `docker compose build dataservice`, and so on.
