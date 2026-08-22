# Task 0.6 — SpringDoc OpenAPI

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Configurar springdoc-openapi com bean `OpenAPI` documentando metadados da API + SecurityScheme `bearerAuth` (JWT) — preparando p/ Sprint 2 sem precisar revisitar.

## Critério de aceite

- [ ] `config/SpringDocOpenApiConfig.java` criado.
- [ ] Bean `OpenAPI` com `Info` (título, descrição, versão, license, contact).
- [ ] SecurityScheme `bearerAuth` tipo HTTP, scheme `bearer`, bearerFormat `JWT`.
- [ ] SecurityRequirement adicionado.
- [ ] Quando app sobe: `http://localhost:8080/docs.html` → Swagger UI funcional.
- [ ] `http://localhost:8080/v3/api-docs` → JSON OpenAPI 3.

## Especificação

```java
package br.com.mauricio.contas.config;

import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
import io.swagger.v3.oas.models.security.SecurityRequirement;
import io.swagger.v3.oas.models.security.SecurityScheme;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SpringDocOpenApiConfig {

    private static final String SECURITY_SCHEME_NAME = "bearerAuth";

    @Bean
    OpenAPI contasApiOpenAPI() {
        return new OpenAPI()
            .info(new Info()
                .title("Contas API")
                .description("API REST de treinamento — Hexagonal + DDD + TDD")
                .version("v1.0")
                .license(new License().name("Apache 2.0"))
                .contact(new Contact().name("Mauricio Chaves").email("feira.nova@gmail.com")))
            .addSecurityItem(new SecurityRequirement().addList(SECURITY_SCHEME_NAME))
            .components(new Components()
                .addSecuritySchemes(SECURITY_SCHEME_NAME,
                    new SecurityScheme()
                        .name(SECURITY_SCHEME_NAME)
                        .type(SecurityScheme.Type.HTTP)
                        .scheme("bearer")
                        .bearerFormat("JWT")
                        .description("JWT Bearer token (entra em vigor na Sprint 2)")));
    }
}
```

## Lembrete sobre Security

Sprint 0 não terá Spring Security ativo (ainda não criamos `SecurityFilterChain`). Spring Security starter adicionado no `pom.xml` vai gerar auto-config que protege todos endpoints com user `user` + senha random no log.

P/ Sprint 0 funcionar (`/actuator/health`, `/docs.html`), você tem 2 opções:

**Opção A (recomendada)** — criar `infrastructure/security/SpringSecurityConfig.java` stub permitAll:

```java
package br.com.mauricio.contas.infrastructure.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.web.SecurityFilterChain;

/**
 * Sprint 0: stub permitAll p/ smoke test.
 * Sprint 2 substitui por config JWT real.
 */
@Configuration
public class SpringSecurityConfig {
    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(c -> c.disable())
            .sessionManagement(s -> s.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(a -> a.anyRequest().permitAll())
            .build();
    }
}
```

**Opção B** — remover `spring-boot-starter-security` do pom.xml por enquanto e adicionar na Sprint 2.

Recomendo **A**. Você já materializa a classe no pacote certo, com Javadoc explicando o débito.

## Passos

1. Criar `SpringDocOpenApiConfig`.
2. Criar `SpringSecurityConfig` stub (Opção A).
3. `./mvnw compile` verde.

## Commit sugerido

```
feat(docs): springdoc openapi config + security scheme JWT

- Bean OpenAPI com info, contact, license
- SecurityScheme bearerAuth registrado (usado a partir da Sprint 2)
- SpringSecurityConfig stub permitAll (debito documentado p/ Sprint 2)
```

## Code Review — o que vou olhar

- SecurityScheme name = "bearerAuth"?
- SecurityRequirement adicionado (senão Swagger UI não mostra botão "Authorize")?
- Javadoc no SpringSecurityConfig deixando claro que é stub?
- Versão exata do springdoc no pom?
