# ROADMAP — Contas API

> **Leia no início de cada sessão.** Visão temporal das sprints, marcos, dependências.

**Última atualização**: 2026-06-15

---

## Linha do Tempo (estimativa de estudo focado)

```
Sprint 0 ─┐ Fundação (1 dia)
          │
Sprint 1 ─┤ Usuários (1 semana)
          │
Sprint 2 ─┤ Auth JWT (3-4 dias)
          │
Sprint 3 ─┤ Conta (4-5 dias)
          │
Sprint 4 ─┤ Transações (1 semana)
          │
Sprint 5 ─┘ Hardening + v1.0 (2-3 dias)

≈ 4-5 semanas de estudo concentrado
```

Datas são guia, não compromisso. Ritmo determinado pelo aluno.

---

## Sprints e Marcos

### Sprint 0 — Fundação 🏗️ (atual)

**Marco**: `./mvnw verify` verde com Postgres em Testcontainers. App sobe. Swagger UI carrega.

**Sai com**:
- Projeto Maven funcional
- Docker Compose Postgres
- Estrutura de pacotes hexagonal
- Configs cross-cutting (Locale, Timezone, JpaAuditing stub)
- AbstractIntegrationTest + smoke IT
- Flyway baseline V1
- SpringDoc OpenAPI configurado

**8 tasks** → ver `sprint-0-fundacao/`

---

### Sprint 1 — Gestão de Usuários 👤

**Pré-requisito**: Sprint 0 fechada.

**Marco**: Endpoint REST `/api/v1/usuarios` funcional (sem auth ainda). IT end-to-end criar → buscar → listar → alterar senha verde.

**Sai com**:
- Domain layer completo p/ Usuário (Aggregate + 3 VOs + port out)
- 4 Use Cases impl em application layer
- JPA adapter + V2 migration
- Controller + DTOs records + ExceptionHandler
- Cobertura ≥ 80% no domain

**14 tasks** → backlog em `sprint-1-usuarios/` (criado quando Sprint 0 fechar)

**Foco didático**:
- Primeiro contato com TDD estrito
- Records em VOs (compact constructor + invariantes)
- Records em DTOs com Bean Validation
- Records em Commands
- Mapper domain ↔ JPA entity ↔ DTO

---

### Sprint 2 — Autenticação JWT 🔐

**Pré-requisito**: Sprint 1 fechada (precisa de UsuarioRepositoryPort).

**Marco**: Fluxo `criar user → login → token → acesso rota protegida` verde via IT.

**Sai com**:
- `SpringSecurityConfig` stateless real (substitui stub da Sprint 0)
- JJWT 0.12.6 wired (TokenProvider, Filter, UserDetailsService)
- RBAC por `@PreAuthorize`
- JpaAuditing agora puxa principal autenticado
- `AuthController` + DTOs records

**9 tasks**

**Foco didático**:
- Spring Security 6 lambda DSL
- JJWT moderno (sem o legado 0.11)
- BCrypt
- Filter chain customizada
- Records p/ JwtClaims, TokenResponse, LoginRequest

---

### Sprint 3 — Domínio Conta 💰

**Pré-requisito**: Sprint 2 fechada (precisa de principal autenticado p/ Auditing).

**Marco**: ADMIN abre conta → CLIENTE consulta saldo via JWT.

**Sai com**:
- `Money` record (VO mais rico do projeto)
- `Conta` aggregate com invariantes (saldo ≥ 0, depositar, debitar, encerrar)
- Domain events records
- 2 Use Cases + JPA adapter + V3 migration
- Controller + DTOs records

**11 tasks**

**Foco didático**:
- VO rico com operações (`Money.somar`, `Money.subtrair`)
- Invariantes no aggregate (não no service)
- Publicação de eventos via port out
- Mapeamento Money → 2 colunas JPA

---

### Sprint 4 — Transações 💸

**Pré-requisito**: Sprint 3 fechada.

**Marco**: Fluxo completo depositar → sacar → transferir → extrato funcional + cenários negativos cobertos.

**Sai com**:
- `sealed interface Transacao permits Deposito, Saque, Transferencia` + records
- 4 Use Cases + adapter + V4 migration
- Controller + DTOs records
- Pattern matching switch sobre Transacao na projeção do extrato
- Cenários negativos cobertos (saldo insuficiente → 422, conta inexistente → 404)

**10 tasks**

**Foco didático**:
- Sealed types Java 17+
- Pattern matching switch Java 21
- Cross-aggregate `@Transactional` (com débito catalogado)
- Idempotência básica

---

### Sprint 5 — Hardening + Release v1.0 🚀

**Pré-requisito**: Sprint 4 fechada.

**Marco**: Tag `v1.0.0` com cobertura, docs e logs estruturados.

**Sai com**:
- JaCoCo ≥ 80% global, ≥ 90% domain
- Swagger UI 100% documentado (descrições, exemplos)
- Logs Logback JSON em profile `prod`
- Health checks customizados
- Profile `prod` separado
- README com setup + ADRs
- Retro escrita

**8 tasks**

**Foco didático**:
- Observabilidade
- Profile separation (dev vs prod)
- Releases versionadas

---

## Dependências Entre Sprints

```
Sprint 0 ─┐
          │
          ▼
Sprint 1 ─┐ usa: configs cross-cutting, Testcontainers, package structure
          │ entrega: UsuarioRepositoryPort, Aggregate Usuario
          ▼
Sprint 2 ─┐ usa: UsuarioRepositoryPort, Usuario
          │ entrega: principal autenticado p/ JpaAuditing real
          ▼
Sprint 3 ─┐ usa: principal autenticado, RBAC
          │ entrega: ContaRepositoryPort, Aggregate Conta
          ▼
Sprint 4 ─┐ usa: Conta + ContaRepositoryPort
          │ entrega: TransacaoRepositoryPort, sealed Transacao
          ▼
Sprint 5 ─┘ usa: tudo
```

Cada sprint tem ~1 marco final que materializa o aprendizado. Não pula sprint.

---

## Sinais de "pronto p/ próxima sprint"

Antes de abrir backlog da próxima sprint, confirme:

- [ ] Todas as tasks da sprint atual marcadas `🟢` no `BACKLOG.md`.
- [ ] `./mvnw verify` verde sem skip.
- [ ] DoD da sprint cumprida (ver `sprint-X-*/README.md`).
- [ ] Cobertura nas classes alteradas ≥ 80%.
- [ ] `STATE.md` atualizado.
- [ ] Retro rápida: o que aprendi, o que custou, o que faria diferente.

---

## Como Claude usa este arquivo

No início de cada sessão, Claude lê:

1. **STATE.md** → estado atual, próxima task.
2. **ROADMAP.md** → onde estamos no arco maior.
3. **PRD.md** (apenas se aluno levantar dúvida de escopo/objetivo).
4. **Task file** da próxima task.

Se ROADMAP mudou de forma estrutural (sprint nova, sprint cancelada), atualize aqui antes de prosseguir.
