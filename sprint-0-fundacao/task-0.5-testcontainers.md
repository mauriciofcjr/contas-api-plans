# Task 0.5 — AbstractIntegrationTest com Testcontainers

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Criar classe base abstrata que sobe Postgres em container via Testcontainers 2.0.5 + `@ServiceConnection` do Spring Boot 4.0.8. Toda IT vai estender essa classe.

## Critério de aceite

- [ ] `src/test/java/br/com/mauricio/contas/AbstractIntegrationTest.java` criado.
- [ ] Anotada com `@SpringBootTest(webEnvironment = RANDOM_PORT)`, `@ActiveProfiles("test")`, `@Testcontainers`.
- [ ] Campo `static final PostgreSQLContainer` com `@Container` + `@ServiceConnection`.
- [ ] Setup RestAssured no `@BeforeEach` (port + baseURI).
- [ ] `src/test/resources/application-test.yml` minimal (ddl-auto validate, flyway enabled).
- [ ] Dependências `spring-boot-testcontainers`, `testcontainers-postgresql` e `testcontainers-junit-jupiter` presentes, sem BOM manual do Testcontainers.

## Especificação

```java
import org.testcontainers.postgresql.PostgreSQLContainer;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@ActiveProfiles("test")
@Testcontainers
public abstract class AbstractIntegrationTest {

    @Container
    @ServiceConnection
    static final PostgreSQLContainer POSTGRES = new PostgreSQLContainer("postgres:16-alpine");

    @LocalServerPort
    int port;

    @BeforeEach
    void setUpRestAssured() {
        io.restassured.RestAssured.port = port;
        io.restassured.RestAssured.baseURI = "http://localhost";
    }
}
```

### application-test.yml

- `spring.jpa.hibernate.ddl-auto: validate`
- `spring.jpa.show-sql: false`
- `spring.flyway.enabled: true`
- `logging.level.org.testcontainers: INFO`

## Por que `@ServiceConnection`?

Spring Boot 4.0.8 detecta o container Postgres anotado com `@ServiceConnection` e configura datasource automaticamente — sem precisar de `@DynamicPropertySource`. Mais limpo.

## Passos

1. Criar classe abstract.
2. Criar `application-test.yml`.
3. (Sem teste concreto ainda — Task 0.7 cria.)

## Dica

- `@Container` + `static` faz o container ser reutilizado entre testes da classe.
- A versão 2.0.5 do Testcontainers vem do dependency management do Spring Boot 4.0.8; não declarar versão nem importar `testcontainers-bom`.
- Em Testcontainers 2.x, usar os artefatos `testcontainers-postgresql` e `testcontainers-junit-jupiter` e importar `org.testcontainers.postgresql.PostgreSQLContainer`.
- Para containers compartilhados entre múltiplas classes de teste, ver "Singleton Container Pattern" (avançado, não necessário Sprint 0).
- A classe atual `org.testcontainers.postgresql.PostgreSQLContainer` não é genérica; não usar `<?>` nem o pacote de compatibilidade depreciado `org.testcontainers.containers`.

## Commit sugerido

```
test(infra): AbstractIntegrationTest com testcontainers postgres

Usa @ServiceConnection (SB 4.0.8) para wire automatico de datasource.
RestAssured configurado no @BeforeEach.
```

## Code Review — o que vou olhar

- Classe `abstract`?
- Container `static final` (reutilizado)?
- `@ServiceConnection` (não `@DynamicPropertySource` legado)?
- Testcontainers 2.0.5 gerenciado pelo parent, sem BOM manual nem módulos 1.x?
- RestAssured setup no `@BeforeEach`?
- `application-test.yml` separado de `application-dev.yml`?
