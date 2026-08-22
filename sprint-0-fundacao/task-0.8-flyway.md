# Task 0.8 — Flyway Baseline V1

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Criar migration baseline `V1__baseline.sql` para Flyway ter algo p/ aplicar no startup. Tabelas reais (`usuarios`, `contas`, `transacoes`) entram em V2/V3/V4 nas próximas sprints.

## Critério de aceite

- [ ] `src/main/resources/db/migration/V1__baseline.sql` existe.
- [ ] Pelo menos 1 statement SQL válido (pode ser `SELECT 1;` ou comentário + statement noop).
- [ ] Naming segue convenção Flyway: `V<versão>__<descrição>.sql` (com 2 underscores).
- [ ] Boot da app aplica V1 sem erro.

## Especificação

`src/main/resources/db/migration/V1__baseline.sql`:

```sql
-- Sprint 0: baseline vazia.
-- Tabelas reais entram em:
--   V2 — Sprint 1 (usuarios)
--   V3 — Sprint 3 (contas)
--   V4 — Sprint 4 (transacoes)
SELECT 1;
```

## Por que isso é necessário?

`spring.flyway.enabled: true` + `baseline-on-migrate: true` exige pelo menos 1 migration no classpath, senão dispara `FlywayException`. Statement `SELECT 1;` é inofensivo e válido.

## Passos

1. Criar diretório `src/main/resources/db/migration/`.
2. Criar `V1__baseline.sql` com conteúdo acima.
3. (Validação ocorre na Task 0.7 quando IT rodar.)

## Dica

- Naming Flyway é estrito: 2 underscores entre versão e descrição (`V1__baseline.sql`, não `V1_baseline.sql`).
- Convenção do projeto: usar `V<N>__<dominio>.sql` em vez de descrição genérica.
- Não use `R__` (repeatable) — só versionadas neste projeto.

## Commit sugerido

```
chore(db): flyway baseline V1 vazia

Statement SELECT 1 como placeholder.
Migrations reais entram nas sprints seguintes.
```

## Code Review — o que vou olhar

- Naming `V1__baseline.sql` (2 underscores)?
- Path exato `src/main/resources/db/migration/`?
- Comentário explicando porque é vazia?
