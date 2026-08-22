# Task 0.7 — Smoke IT `/actuator/health`

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Provar end-to-end que: app sobe → conecta no Postgres (Testcontainers) → Actuator responde `200 UP`. É o sinal verde da Sprint 0.

## Critério de aceite

- [ ] `src/test/java/br/com/mauricio/contas/HealthCheckIT.java` criado, estendendo `AbstractIntegrationTest`.
- [ ] 1 teste: `GET /actuator/health` → status 200, body `status=UP`.
- [ ] `./mvnw verify` verde (rode até confirmar).
- [ ] Tempo total < 60s (boot + container).

## Especificação

```java
package br.com.mauricio.contas;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static io.restassured.RestAssured.given;
import static org.hamcrest.Matchers.equalTo;

@DisplayName("Smoke: aplicação sobe e /actuator/health responde UP")
class HealthCheckIT extends AbstractIntegrationTest {

    @Test
    @DisplayName("GET /actuator/health → 200 com status=UP")
    void healthEndpointReturnsUp() {
        given()
            .when().get("/actuator/health")
            .then()
            .statusCode(200)
            .body("status", equalTo("UP"));
    }
}
```

## Passos

1. Garantir que Docker Desktop está rodando (`docker ps`).
2. Criar `HealthCheckIT.java`.
3. Rodar `./mvnw verify`.
4. Esperar pull da imagem `postgres:16-alpine` (primeira vez ~20s).
5. Validar:
   - `[INFO] Tests run: 1, Failures: 0, Errors: 0`
   - `BUILD SUCCESS`
   - JaCoCo report gerado em `target/site/jacoco/index.html`.

## Troubleshooting

| Sintoma | Causa | Solução |
|---|---|---|
| `Could not find a valid Docker environment` | Docker Desktop parado | `open -a Docker` e aguardar |
| `Connection refused` na porta randomica | `@LocalServerPort` não injetado | conferir `@SpringBootTest(webEnvironment = RANDOM_PORT)` |
| Flyway falha "no migration found" | falta V1 | terminar Task 0.8 antes |
| `ddl-auto: validate` falha | entidade no código sem coluna no schema | nesta Sprint 0 ainda não há entity — não deve ocorrer |

## Commit sugerido

```
test(smoke): HealthCheckIT valida app sobe e /actuator/health responde UP

End-to-end via Testcontainers + RestAssured.
Sela DoD da Sprint 0.
```

## Code Review — o que vou olhar

- Classe estende `AbstractIntegrationTest`?
- `@DisplayName` PT-BR descritivo?
- Assertion mínima e clara (`statusCode` + `body status=UP`)?
- Sem mock — é IT real?
- `./mvnw verify` verde no seu output?
