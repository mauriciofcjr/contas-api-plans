# PRD — Contas API (Training)

> **Documento vivo**. Última atualização: 2026-07-13.

## 1. Visão

`contas-api` é um projeto de **treinamento corporativo** que entrega uma API REST de **conta corrente bancária simplificada** (Banking-lite), construída sob os padrões **Hexagonal Architecture + DDD + TDD + SOLID + Clean Code**, com foco didático em **Java 21 Records**, JWT, Spring Boot 4.0 e PostgreSQL.

Não é um produto comercial. É o veículo de aprendizado do desenvolvedor para internalizar:
- Arquitetura Hexagonal (Ports & Adapters)
- TDD estrito (Red → Green → Refactor)
- Modelagem por Aggregates, Value Objects e Domain Events
- Uso correto de Java Records em camadas de fronteira (DTOs, VOs, Commands, Events)
- Segurança stateless com JWT
- Persistência com JPA + Flyway + Testcontainers

## 2. Problema

Cursos tradicionais ensinam Spring Boot em estilo CRUD + 3 camadas (controller → service → repository) com entities anêmicas. O desenvolvedor termina sem saber:

- Onde colocar regra de negócio? (resposta certa: dentro do aggregate, não no service)
- Como isolar domínio de framework? (resposta certa: hexagonal, sem `@Entity` no domínio)
- Como praticar TDD em cenário Spring? (Testcontainers + WebMvcTest + JUnit5)
- Quando usar record vs classe? (record para imutáveis na fronteira; classe para aggregate mutável)

`contas-api` é o playground para fechar essas lacunas.

## 3. Objetivos

### 3.1 Objetivos de aprendizado (primários)

| # | Objetivo | Como mediremos |
|---|---|---|
| O1 | Desenvolvedor escreve teste antes do código em **100%** das tasks de produção | Histórico git: commit `test: red` precede `feat: green` |
| O2 | Desenvolvedor mantém `domain/` sem imports de Spring/JPA | Checklist code review + smoke grep no CI |
| O3 | Desenvolvedor usa records corretamente (DTO/VO/Command/Event) e classes para aggregates | Code review por task |
| O4 | Cobertura JaCoCo ≥ 80% global, ≥ 90% no `domain/` | Relatório JaCoCo ao final da Sprint 5 |
| O5 | Desenvolvedor consegue justificar cada decisão arquitetural (entrevista de fechamento) | Sessão de retro Sprint 5 |

### 3.2 Objetivos funcionais (entregáveis)

- **F1** API REST de gestão de usuários (criar, buscar, listar, alterar senha)
- **F2** Autenticação JWT stateless com BCrypt e RBAC (ADMIN / CLIENTE)
- **F3** API REST de gestão de contas (abrir, consultar saldo)
- **F4** API REST de transações (depositar, sacar, transferir, extrato)
- **F5** Documentação OpenAPI 3 navegável (Swagger UI)
- **F6** Testes de integração end-to-end com Postgres real (Testcontainers)

## 4. Escopo

### 4.1 In Scope (v1.0)

- CRUD de usuários com autenticação JWT
- Aggregate `Conta` com invariante de saldo não-negativo
- Transações: depósito, saque, transferência (atômica), extrato paginado
- Autorização por roles (ADMIN vê tudo; CLIENTE vê só si mesmo)
- Domain events publicados via `ApplicationEventPublisher`
- JPA Auditing (created_by/modified_by puxado do principal)
- Locale pt-BR, Timezone America/Sao_Paulo

### 4.2 Out of Scope (v1.0)

- ❌ Frontend (apenas Swagger UI)
- ❌ Multi-tenancy
- ❌ Saga / Outbox pattern (cross-aggregate fica `@Transactional` simples)
- ❌ Event Sourcing
- ❌ Microsserviços (é modular monolith)
- ❌ Caching (Redis, etc.)
- ❌ Rate limiting
- ❌ Logs estruturados em JSON (opcional Sprint 5)
- ❌ Deploy em cloud (apenas local + Docker Compose)
- ❌ CI/CD (opcional Sprint 5)
- ❌ Internationalization de mensagens de erro além de pt-BR

### 4.3 Non-Goals

- Não é um sistema bancário real. Não há compliance (PCI, LGPD, BACEN). Não use em produção.
- Não é benchmark de performance. Decisões priorizam clareza didática sobre throughput.

## 5. Personas

| Persona | Descrição | Necessidade |
|---|---|---|
| **Desenvolvedor-aluno** (Mauricio) | Dev em treinamento Java/Spring, quer aprender DDD + Hexagonal + TDD em produção-like | Backlog estruturado, dicas TDD, code review por task |
| **PO/Tech Lead (IA)** | Claude atuando como mentor | Histórico do projeto, regras de negócio, convenções, estado atual |
| **ADMIN da API** | Usuário fictício que gerencia outros usuários e contas | Endpoints autorizados, RBAC funcional |
| **CLIENTE da API** | Usuário fictício que possui conta e movimenta saldo | Login, consulta de saldo, transações próprias |

## 6. Casos de Uso Principais

### 6.1 Gestão de Usuários (Sprint 1)

- UC1.1 Criar usuário (qualquer pessoa, sem auth) — username único, senha 6 chars
- UC1.2 Buscar usuário por id (ADMIN: qualquer; CLIENTE: só self)
- UC1.3 Listar usuários paginado (apenas ADMIN)
- UC1.4 Alterar senha (apenas self, autenticado)

### 6.2 Autenticação (Sprint 2)

- UC2.1 Login retorna JWT (24h validade)
- UC2.2 Token validado em filtro p/ rotas protegidas
- UC2.3 401 quando token inválido / expirado
- UC2.4 403 quando role insuficiente

### 6.3 Conta (Sprint 3)

- UC3.1 Abrir conta (ADMIN cria p/ um usuário) — número único, saldo inicial 0
- UC3.2 Consultar saldo (dono ou ADMIN)

### 6.4 Transações (Sprint 4)

- UC4.1 Depositar valor positivo numa conta
- UC4.2 Sacar valor (rejeita se saldo insuficiente)
- UC4.3 Transferir conta origem → destino (atômico)
- UC4.4 Listar extrato paginado por período

## 7. Requisitos Não-Funcionais

| RN | Descrição |
|---|---|
| RN1 | Timezone fixo `America/Sao_Paulo` em todas as datas |
| RN2 | Locale fixo `pt-BR` em mensagens e parsing numérico |
| RN3 | Stateless: sem sessão HTTP; JWT como única forma de auth |
| RN4 | Senha sempre armazenada com BCrypt (cost 10) |
| RN5 | Migrations Flyway versionadas (`V<N>__*.sql`) |
| RN6 | `ddl-auto: validate` em todos profiles (nunca `update`/`create`) |
| RN7 | `open-in-view: false` |
| RN8 | Testes de integração com Postgres **real** (Testcontainers), não H2 |
| RN9 | Build via Maven wrapper (`./mvnw`) |

## 8. Decisões Arquiteturais (ADR sintético)

| ID | Decisão | Justificativa |
|---|---|---|
| ADR-001 | Hexagonal puro (Ports & Adapters) | Disciplina máxima de isolamento de domínio; valor didático alto |
| ADR-002 | Domain POJO + JPA Entity separados + Mapper | Cumpre regra "domain sem JPA"; custo aceito |
| ADR-003 | Java Records p/ DTO/VO/Command/Event | Imutabilidade na fronteira; exercitar feature LTS |
| ADR-004 | Aggregate como classe mutável (não record) | Aggregates têm estado evolutivo + identidade por id |
| ADR-005 | `sealed interface Transacao permits Deposito, Saque, Transferencia` | Exaustividade em pattern matching switch |
| ADR-006 | Testcontainers Postgres real (não H2) | Evita divergências H2 vs Postgres (UUID, jsonb, etc.) |
| ADR-007 | ModelMapper p/ web ↔ domain | Reduz boilerplate; aceitar mapeamento manual em casos complexos |
| ADR-008 | Transferência cross-aggregate via `@Transactional` simples | Saga/Outbox seria over-engineering didático |
| ADR-009 | JJWT 0.12.x (não 0.11.x) | API moderna `Jwts.builder().signWith(key, alg)` |
| ADR-010 | Spring Boot 4.0.8 (não 3.3.x) | Spring Framework 7 + Jakarta EE 11 + Jackson 3; exige springdoc-openapi 3.0.x; Java 21 mantido (baseline SB4 = Java 17) |
| ADR-011 | Testcontainers 2.0.5 gerenciado pelo Spring Boot | Evita BOM manual incompatível; usa os módulos 2.x `testcontainers-postgresql` e `testcontainers-junit-jupiter` e mantém compatibilidade com Docker Engine 29 |

## 9. Métricas de Sucesso

| Métrica | Alvo | Quando medir |
|---|---|---|
| Tasks executadas com TDD evidente (commit RED → GREEN) | 100% | Final de cada Sprint |
| Cobertura global JaCoCo | ≥ 80% | Final Sprint 5 |
| Cobertura `domain/` JaCoCo | ≥ 90% | Final Sprint 5 |
| Sprints concluídas no prazo (semanal) | 5/5 | Final Sprint 5 |
| Code reviews com 🟢 na primeira rodada | ≥ 60% | Acumulativo |
| Desenvolvedor consegue explicar todas as ADRs sem consulta | Sim | Retro Sprint 5 |

## 10. Glossário

| Termo | Significado |
|---|---|
| Aggregate | Cluster de objetos de domínio tratado como unidade; tem 1 raiz com identidade |
| Value Object (VO) | Objeto definido por seus valores, sem identidade; imutável |
| Port | Interface no domínio que descreve uma dependência (in = use case; out = repository, publisher) |
| Adapter | Implementação infraestrutural de uma port |
| Use Case | Caso de uso de aplicação; orquestra aggregates + ports |
| Command | Record de input p/ um use case |
| Event | Record imutável que descreve algo que aconteceu no domínio |
| TDD | Test-Driven Development: Red → Green → Refactor |

## 11. Referências

- Park-API (referência layered MVC): `/Users/mauriciochaves/Documents/workspaces/workspace-estudos/park-api/`
- DDD Reference, Eric Evans
- Implementing Domain-Driven Design, Vaughn Vernon
- Hexagonal Architecture, Alistair Cockburn
