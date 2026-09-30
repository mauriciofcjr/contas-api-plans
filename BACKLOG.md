# Backlog — Contas API

Domínio: **Banking-lite** (Conta, Transação, Transferência) + Usuários + Auth JWT.

## Status Legend

🟢 done · 🟡 in progress · ⚪ pending · 🔒 blocked

---

## EPIC 0 — Fundação (Sprint 0)

| # | Task | Status |
|---|---|---|
| 0.1 | [Bootstrap projeto Maven](sprint-0-fundacao/task-0.1-bootstrap.md) | 🟢 |
| 0.2 | [Docker Compose Postgres + configs YAML](sprint-0-fundacao/task-0.2-docker-compose.md) | ⚪ |
| 0.3 | [Estrutura pacotes hexagonal](sprint-0-fundacao/task-0.3-packages.md) | ⚪ |
| 0.4 | [Configs Locale + Timezone + JpaAuditing](sprint-0-fundacao/task-0.4-cross-configs.md) | ⚪ |
| 0.5 | [AbstractIntegrationTest com Testcontainers](sprint-0-fundacao/task-0.5-testcontainers.md) | ⚪ |
| 0.6 | [SpringDoc OpenAPI](sprint-0-fundacao/task-0.6-springdoc.md) | ⚪ |
| 0.7 | [Smoke IT /actuator/health](sprint-0-fundacao/task-0.7-smoke-it.md) | ⚪ |
| 0.8 | [Flyway baseline V1](sprint-0-fundacao/task-0.8-flyway.md) | ⚪ |

**DoD Sprint 0**: `./mvnw verify` verde com Postgres em Testcontainers; Swagger acessível; CI local OK.

---

## EPIC 1 — Gestão de Usuários (Sprint 1) — aberto após Sprint 0 fechar

| # | Task | Status |
|---|---|---|
| 1.1 | VO `Email` (record + validação) | 🔒 |
| 1.2 | VO `Senha` (record, 6 chars) | 🔒 |
| 1.3 | VO `UsuarioId` (record UUID) | 🔒 |
| 1.4 | Aggregate `Usuario` | 🔒 |
| 1.5 | Port `UsuarioRepositoryPort` | 🔒 |
| 1.6 | `CriarUsuarioUseCase` + command record | 🔒 |
| 1.7 | `BuscarUsuarioPorIdUseCase` | 🔒 |
| 1.8 | `ListarUsuariosUseCase` paginado | 🔒 |
| 1.9 | `AlterarSenhaUseCase` | 🔒 |
| 1.10 | JPA adapter + Mapper + V2 migration | 🔒 |
| 1.11 | DTOs records + Bean Validation | 🔒 |
| 1.12 | UsuarioController + WebMapper | 🔒 |
| 1.13 | ApiExceptionHandler + ErrorMessage record | 🔒 |
| 1.14 | IT end-to-end fluxo completo | 🔒 |

---

## EPIC 2 — Autenticação JWT (Sprint 2) — 🔒

(detalhes abertos quando Sprint 1 fechar)

## EPIC 3 — Domínio Conta (Sprint 3) — 🔒

## EPIC 4 — Transações (Sprint 4) — 🔒

## EPIC 5 — Hardening (Sprint 5) — 🔒
