# STATE — Contas API

> **Para agentes de IA.** Leia este arquivo no início de cada sessão para saber o estado atual do projeto sem precisar varrer git/filesystem. Mantenha atualizado ao final de cada task.

**Última atualização**: 2026-06-15 09:30 BRT
**Atualizado por**: Claude (PO/Tech Lead)

---

## Status Geral

| Item | Valor |
|---|---|
| Sprint atual | **Sprint 0 — Fundação** |
| Progresso Sprint 0 | 0 / 8 tasks concluídas |
| Próxima task | **Task 0.1 — Bootstrap projeto Maven** |
| Projeto criado em disco? | ❌ Não. `contas-api/` ainda não existe. Task 0.1 cria. |
| Build status | N/A (sem código ainda) |
| Cobertura | N/A |

---

## Workflow Acordado

| Papel | Responsável |
|---|---|
| Implementação 100% | Mauricio (humano) |
| PO / Tech Lead / Reviewer | Claude (IA) |
| Criar/manter task files em `contas-api-plans/` | Claude |
| Escrever código, testes, commits | Mauricio |
| Code review por task | Claude |

Mauricio implementa lendo os arquivos `task-X.Y-*.md`. Cola código no chat. Claude revisa. Aprovação destrava próxima task.

---

## Diretórios

| Path | Conteúdo |
|---|---|
| `/Users/mauriciochaves/Documents/workspaces/workspace-estudos/projeto-contas/contas-api/` | Projeto Java (ainda não criado) |
| `/Users/mauriciochaves/Documents/workspaces/workspace-estudos/projeto-contas/contas-api-plans/` | Backlog + task files + docs |
| `/Users/mauriciochaves/Documents/workspaces/workspace-estudos/park-api/` | Projeto de referência (consultar padrões) |
| `/Users/mauriciochaves/.claude/plans/preciso-que-vc-seja-async-deer.md` | Plano consolidado original |

---

## Stack Definido

- **Java 21** (Adoptium Temurin instalado em `/Users/mauriciochaves/jdks/jdk-21.0.11+10/...`)
- **Maven 3.9.15** (em `/Users/mauriciochaves/maven/bin/mvn`)
- **Spring Boot 4.0.8**
- **PostgreSQL 16** via Docker Compose
- **Testcontainers 1.20.1** + `@ServiceConnection`
- **JJWT 0.12.6**
- **ModelMapper 3.2.1**
- **springdoc-openapi 3.0.3** (linha 3.x exigida por Spring Boot 4 / Spring Framework 7)
- **Jackson 3** (default no Spring Boot 4; namespace `tools.jackson.*`)
- **Lombok** (apenas em entities/adapters; proibido em records)

## Ambiente Local

| Recurso | Status confirmado em sessão anterior |
|---|---|
| Docker Desktop | Instalado, daemon precisa ser iniciado manualmente (`open -a Docker`) |
| Maven | OK |
| Java 21 | OK |

---

## Decisões Tomadas (com vc presente)

| Data | Decisão | Por quê |
|---|---|---|
| 2026-06-15 | Domínio: Banking-lite (Conta, Transação, Transferência) | Bom p/ treinar VOs (Money), invariantes (saldo), eventos |
| 2026-06-15 | Hexagonal puro (Ports & Adapters) | Disciplina máxima de isolamento |
| 2026-06-15 | Java 21 + SB 4.0 + Testcontainers | LTS + recursos modernos (records, sealed) |
| 2026-06-15 | Nome `contas-api` / pkg `br.com.mauricio.contas` | Default sugerido + aceito |
| 2026-06-15 | Workflow: PO entrega task em `.md`, dev implementa, PO revisa | Aluno quer 100% prática |
| 2026-06-15 | Tasks em `contas-api-plans/sprint-*/task-*.md` | Estrutura por sprint |
| 2026-06-15 | Projeto inicial criado por engano pelo Claude → apagado | Aluno só quer escrever código ele mesmo |
| 2026-07-13 | Upgrade Spring Boot 3.3.9 → 4.0.8 | Pedido do aluno; puxa Spring Framework 7 + Jakarta EE 11 + Jackson 3; springdoc bumped 2.5.0 → 3.0.3 |

---

## Backlog Resumido

| Sprint | Status | Tasks concluídas |
|---|---|---|
| Sprint 0 — Fundação | ⚪ pendente | 0/8 |
| Sprint 1 — Usuários | 🔒 bloqueada | 0/14 |
| Sprint 2 — Auth JWT | 🔒 bloqueada | 0/9 |
| Sprint 3 — Conta | 🔒 bloqueada | 0/11 |
| Sprint 4 — Transações | 🔒 bloqueada | 0/10 |
| Sprint 5 — Hardening | 🔒 bloqueada | 0/8 |

Ver `BACKLOG.md` p/ lista detalhada.

---

## Decisões em Aberto

(nenhuma)

---

## Riscos & Débitos

| ID | Item | Severidade | Quando resolver |
|---|---|---|---|
| R1 | `SpringSecurityConfig` na Sprint 0 será stub `permitAll` | 🟡 baixa | Sprint 2 (substitui por JWT real) |
| R2 | ModelMapper 3.x tem suporte experimental a records | 🟡 média | Validar Sprint 1; fallback é mapper manual |
| R3 | `TransferirUseCase` cross-aggregate vai usar `@Transactional` simples | 🟡 média | Aceitar; refatorar saga/outbox em Sprint 5 se houver tempo |
| R4 | Testcontainers requer Docker Desktop ligado | 🟢 baixa | Dev abre Docker antes de rodar `./mvnw verify` |
| R5 | Spring Boot 4 troca default p/ Jackson 3 (`tools.jackson.*`); `jjwt-jackson` 0.12.6 usa Jackson 2 (`com.fasterxml.jackson.*`) | 🟡 média | Validar Task 0.1: se conflito, manter Jackson 2 no classpath p/ JJWT ou usar `jjwt-gson` |

---

## Convenções Importantes (resumo — full em `CONVENTIONS.md`)

- Naming PT-BR p/ entidades de domínio (`Usuario`, `Conta`).
- Use case interface: `<Verbo><Subst>UseCase` em `port.in`.
- Use case impl: `<Verbo><Subst>Service` em `application/`.
- Adapter persistência: `<Subst>PersistenceAdapter` em `infrastructure/persistence/`.
- DTO: `<Verbo><Subst>Request|Response` em `infrastructure/web/.../dto/`.
- Test unit: `*Test.java`. Test IT: `*IT.java`.
- Commit: Conventional Commits (`test: red — ...`, `feat: green — ...`, `refactor: ...`).

---

## Como atualizar este arquivo

Ao final de qualquer task / sessão relevante, edite:

1. **Data** (topo).
2. **Status Geral** (sprint, progresso, próxima task, build status, cobertura).
3. **Decisões Tomadas** se houver nova ADR.
4. **Backlog Resumido** se sprint mudou.
5. **Riscos & Débitos** se novo item ou item resolvido.

Não escreva narrativa longa. Tabelas + bullets. Pense em "diff readable" — agente futuro lê isso em 30s.
