# Sprint 0 — Fundação

## Objetivo

Bootstrap do projeto + infra local + configs cross-cutting. Sem código de domínio. Sem TDD ainda (não há lógica). Apenas teste smoke de integração comprovando que a aplicação sobe e responde `200 UP`.

## Definition of Done (Sprint)

- [ ] `./mvnw verify` verde com Postgres em Testcontainers.
- [ ] `docker compose up -d` sobe Postgres local.
- [ ] Swagger UI acessível em `http://localhost:8080/docs.html`.
- [ ] Estrutura de pacotes hexagonal materializada (mesmo vazia).
- [ ] Flyway com baseline V1.

## Tasks

| # | Task | Bloqueia |
|---|---|---|
| 0.1 | [Bootstrap projeto Maven](task-0.1-bootstrap.md) | 0.2, 0.3 |
| 0.2 | [Docker Compose Postgres + configs YAML](task-0.2-docker-compose.md) | 0.5 |
| 0.3 | [Estrutura pacotes hexagonal](task-0.3-packages.md) | 0.4, 0.6 |
| 0.4 | [Configs Locale + Timezone + JpaAuditing](task-0.4-cross-configs.md) | — |
| 0.5 | [AbstractIntegrationTest com Testcontainers](task-0.5-testcontainers.md) | 0.7 |
| 0.6 | [SpringDoc OpenAPI](task-0.6-springdoc.md) | — |
| 0.7 | [Smoke IT /actuator/health](task-0.7-smoke-it.md) | encerra sprint |
| 0.8 | [Flyway baseline V1](task-0.8-flyway.md) | — |

## Ordem sugerida

`0.1 → 0.2 → 0.3 → 0.4 → 0.8 → 0.6 → 0.5 → 0.7`

Tasks 0.4, 0.6, 0.8 são independentes — pode reordenar.
