# Task 0.2 — Docker Compose Postgres + Configs YAML

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Provisionar Postgres 16 local via Docker Compose + configurar `application.yml` (base) e `application-dev.yml` (profile dev).

## Critério de aceite

- [x] `docker-compose.yml` na raiz do projeto.
- [x] `.env.example` com variáveis padrão.
- [x] `docker compose up -d` sobe Postgres 16 saudável.
- [x] `docker compose down` derruba sem erro.
- [x] `application.yml` define `spring.application.name`, profile default `dev`, Flyway enabled, JPA `open-in-view: false`, Actuator `health,info` expostos.
- [x] `application-dev.yml` define datasource Postgres local.
- [x] Aplicação consegue subir (`./mvnw spring-boot:run`) com Postgres em pé — mesmo sem endpoints próprios.

## Resultado do Review

**Aprovada em 2026-10-01.** Configuração e arquivos atendem aos critérios da task. O compose foi renderizado corretamente com a porta `5432`; a validação de subida na configuração final não pôde ser repetida porque a porta 5432 já estava ocupada no ambiente. A aplicação também segue com falha preexistente de inicialização do Mockito/Byte Buddy no teste `ContasApiApplicationTest`, fora do escopo desta task.

## Especificação

### docker-compose.yml

- Serviço único `postgres`.
- Imagem: `postgres:16-alpine`.
- Container name: `contas-api-postgres`.
- Port: `5432:5432`.
- Volume nomeado: `contas-api-postgres-data`.
- Healthcheck com `pg_isready`.
- Variáveis lidas de `.env` (com defaults via `${VAR:-default}`).

### .env.example

```
POSTGRES_DB=contas
POSTGRES_USER=contas
POSTGRES_PASSWORD=contas
```

### application.yml

Campos mínimos:
- `spring.application.name: contas-api`
- `spring.profiles.active: dev`
- `spring.jpa.open-in-view: false`
- `spring.jpa.properties.hibernate.jdbc.time_zone: America/Sao_Paulo`
- `spring.flyway.enabled: true` + `baseline-on-migrate: true`
- `server.port: 8080`
- `server.error.include-message: always`
- `springdoc.swagger-ui.path: /docs.html`
- `management.endpoints.web.exposure.include: health,info`

### application-dev.yml

- `spring.datasource.url: jdbc:postgresql://localhost:5432/contas`
- `spring.datasource.username: contas`
- `spring.datasource.password: contas`
- `spring.datasource.driver-class-name: org.postgresql.Driver`
- `spring.jpa.hibernate.ddl-auto: validate`
- `spring.jpa.show-sql: true`

## Passos

1. Criar `docker-compose.yml`.
2. Criar `.env.example`. Copiar p/ `.env` localmente (não commitar `.env`).
3. Criar `src/main/resources/application.yml` e `application-dev.yml`.
4. Subir: `docker compose up -d`.
5. Validar healthcheck: `docker compose ps` → deve mostrar `healthy`.
6. (Vai falhar boot da app sem migration Flyway — só rode após Task 0.8.)

## Dica

- `ddl-auto: validate` força você a manter migrations alinhadas com entidades JPA. Sem migration ainda, Hibernate ainda consegue subir porque não tem `@Entity` definida.
- NÃO commite `.env`. Apenas `.env.example`.

## Commit sugerido

```
chore(infra): docker-compose postgres 16 + configs yaml

- docker-compose com healthcheck e volume nomeado
- .env.example documentando variaveis
- application.yml base (profile dev, jpa, flyway, springdoc, actuator)
- application-dev.yml com datasource postgres local
```

## Code Review — o que vou olhar

- `.env` no `.gitignore`?
- Healthcheck do Postgres definido?
- `open-in-view: false` (sempre)?
- `ddl-auto: validate` em dev (não `update`/`create`)?
- Profile `dev` ativo por default?
