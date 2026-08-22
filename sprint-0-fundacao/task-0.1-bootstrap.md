# Task 0.1 — Bootstrap projeto Maven

## Epic / Sprint

EPIC 0 — Fundação · Sprint 0

## Objetivo

Criar projeto Maven `contas-api` com Spring Boot 4.0.8 + Java 21 + todas as dependências necessárias p/ as próximas sprints.

## Critério de aceite

- [ ] Diretório `/Users/mauriciochaves/Documents/workspaces/workspace-estudos/projeto-contas/contas-api/` existe.
- [ ] `pom.xml` válido com Java 21, SB 4.0.8, deps abaixo.
- [ ] Classe principal `ContasApiApplication` em `br.com.mauricio.contas`.
- [ ] `mvnw` wrapper gerado (`mvn -N wrapper:wrapper -Dmaven=3.9.9`).
- [ ] `.gitignore` razoável.
- [ ] `./mvnw -DskipTests package` verde.

## Dependências (pom.xml)

**Coordenadas**:
```
groupId: br.com.mauricio
artifactId: contas-api
version: 0.0.1-SNAPSHOT
java.version: 21
```

**Parent**:
```
spring-boot-starter-parent 4.0.8
```

**Starters Spring**:
- `spring-boot-starter-web`
- `spring-boot-starter-validation`
- `spring-boot-starter-data-jpa`
- `spring-boot-starter-security`
- `spring-boot-starter-actuator`
- `spring-boot-devtools` (runtime, optional)

**Persistence**:
- `org.flywaydb:flyway-core`
- `org.flywaydb:flyway-database-postgresql`
- `org.postgresql:postgresql` (runtime)

**JWT** (versão `0.12.6`):
- `io.jsonwebtoken:jjwt-api`
- `io.jsonwebtoken:jjwt-impl` (runtime)
- `io.jsonwebtoken:jjwt-jackson` (runtime)

**Mapping / OpenAPI / Lombok**:
- `org.modelmapper:modelmapper:3.2.1`
- `org.springdoc:springdoc-openapi-starter-webmvc-ui:3.0.3`
- `org.projectlombok:lombok` (optional)

**Testes** (escopo `test`):
- `spring-boot-starter-test`
- `spring-security-test`
- `spring-boot-testcontainers`
- `org.testcontainers:postgresql`
- `org.testcontainers:junit-jupiter`
- `io.rest-assured:rest-assured:5.5.0`

**dependencyManagement**: importar `org.testcontainers:testcontainers-bom:1.20.1`.

**Plugins build**:
- `spring-boot-maven-plugin` (excluir lombok)
- `maven-surefire-plugin` — include `**/*Test.java`, exclude `**/*IT.java`
- `maven-failsafe-plugin` — include `**/*IT.java`, goals integration-test + verify
- `org.jacoco:jacoco-maven-plugin:0.8.12` — prepare-agent + report (phase verify)

## Passos

1. `mkdir -p contas-api/src/{main,test}/{java/br/com/mauricio/contas,resources}`
2. Criar `pom.xml` (vc escreve do zero — não copia de outro projeto sem entender).
3. Criar classe `ContasApiApplication` com `@SpringBootApplication` e `main(String[] args)`.
4. Criar `.gitignore` (alvos: `target/`, `.idea/`, `.vscode/`, `*.iml`, `.DS_Store`, `*.log`, `.env`).
5. Gerar wrapper:
   ```bash
   cd contas-api
   mvn -N wrapper:wrapper -Dmaven=3.9.9
   ```
6. Validar:
   ```bash
   ./mvnw -DskipTests package
   ```

## Dicas

- `ContasApiApplication` é classe pública, não record.
- Lombok será usado APENAS em entities JPA e adapters mutáveis. Não em records.
- Sem implementar nenhuma config nesta task — só bootstrap.
- **Spring Boot 4 = Jackson 3 default** (`tools.jackson.*`). `jjwt-jackson` 0.12.6 depende de Jackson 2 (`com.fasterxml.jackson.*`). Rode `./mvnw dependency:tree` e confirme que Jackson 2 entra transitivo via JJWT; se `NoClassDefFoundError` de `com.fasterxml`, fixe `jackson-databind` 2.x no classpath ou troque p/ `jjwt-gson`. Ver risco R5 em `STATE.md`. (Jackson do Spring MVC = Jackson 3, não afeta JJWT.)

## Commit sugerido

```
chore(setup): bootstrap projeto maven contas-api

- Spring Boot 4.0.8, Java 21
- starters: web, validation, data-jpa, security, actuator
- deps: postgresql, flyway, jjwt 0.12.6, modelmapper, springdoc 3.0
- testes: spring-boot-test, testcontainers postgres, restassured
- plugins: surefire (*Test), failsafe (*IT), jacoco
```

## Code Review — o que vou olhar

- Versões exatas das deps?
- Sem dep órfã (ex.: H2, webflux que não vamos usar)?
- Surefire/Failsafe separando unit (*Test) de integração (*IT)?
- `mvnw` versionado?
- `ContasApiApplication` minimal (sem `@ComponentScan`/`@EnableXxx` desnecessário)?
