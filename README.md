# Contas API — Plans

Backlog de treinamento p/ projeto `contas-api` (Java 21 + Spring Boot 4.0 + Hexagonal + DDD + TDD).

## Papéis

| Papel | Quem | Responsabilidade |
|---|---|---|
| Implementador | Você (Mauricio) | 100% código, testes, commits |
| Product Owner / Tech Lead / Professor | Claude | Backlog, tasks, code review, dúvidas |

## Estrutura

```
contas-api-plans/
├── README.md                   ← este arquivo
├── BACKLOG.md                  ← índice geral
├── CONVENTIONS.md              ← regras de código, naming, TDD
├── REVIEW-CHECKLIST.md         ← critérios de revisão
├── sprint-0-fundacao/
│   ├── README.md               ← objetivo + DoD da Sprint
│   ├── task-0.1-bootstrap.md
│   ├── task-0.2-docker-compose.md
│   └── …
├── sprint-1-usuarios/
└── …
```

## Workflow por Task

1. **PO entrega task** — abre arquivo `task-X.Y-*.md`, lê objetivo + critério de aceite + dicas TDD.
2. **RED** — escreve teste primeiro. Roda. Vermelho. Commit `test: red — <task>`.
3. **GREEN** — implementa mínimo. Verde. Commit `feat: green — <task>`.
4. **REFACTOR** — se aplicável. Commit `refactor: <task>`.
5. **Code review** — cola diff / código no chat. PO aplica checklist.
6. **Aprovado** → próxima task. **Reprovado** → ajusta + repete.

## Regra de ouro TDD

> Se escrever código de produção antes do teste, **deleta e recomeça**.

## Localização do projeto

```
/Users/mauriciochaves/Documents/workspaces/workspace-estudos/projeto-contas/contas-api/
```

(ainda não criado — Task 0.1 cria)
