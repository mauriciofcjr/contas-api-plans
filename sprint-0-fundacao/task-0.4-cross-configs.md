# Task 0.4 — Configs Locale + Timezone + JpaAuditing

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Forçar Locale `pt-BR` e Timezone `America/Sao_Paulo` no startup + habilitar JPA Auditing com `AuditorAware` stub (retorna `"system"` até Sprint 2).

## Critério de aceite

- [ ] `config/LocaleConfig.java` definindo `Locale.setDefault(Locale.forLanguageTag("pt-BR"))` via `@PostConstruct`.
- [ ] `config/TimezoneConfig.java` definindo `TimeZone.setDefault(TimeZone.getTimeZone("America/Sao_Paulo"))` via `@PostConstruct`.
- [ ] `config/JpaAuditingConfig.java` anotado com `@EnableJpaAuditing(auditorAwareRef = "auditorProvider")` e bean `AuditorAware<String>`.
- [ ] `AuditorAware` retorna `"system"` quando não há principal autenticado, ou `auth.getName()` quando há.
- [ ] `./mvnw compile` verde.

## Especificação

### LocaleConfig

```java
package br.com.mauricio.contas.config;

import jakarta.annotation.PostConstruct;
import org.springframework.context.annotation.Configuration;
import java.util.Locale;

@Configuration
public class LocaleConfig {
    @PostConstruct
    public void init() {
        Locale.setDefault(Locale.forLanguageTag("pt-BR"));
    }
}
```

### TimezoneConfig

Análogo, mas `TimeZone.setDefault(TimeZone.getTimeZone("America/Sao_Paulo"))`.

### JpaAuditingConfig

Esqueleto (vc preenche):
- Anotada com `@Configuration` + `@EnableJpaAuditing(auditorAwareRef = "auditorProvider")`.
- Bean `@Bean AuditorAware<String> auditorProvider()`.
- Lógica: pega `SecurityContextHolder.getContext().getAuthentication()`. Se null, não autenticado, ou principal "anonymousUser" → `Optional.of("system")`. Senão → `Optional.of(auth.getName())`.

## Passos

1. Criar as 3 classes em `br.com.mauricio.contas.config`.
2. Compilar.
3. (Não rode app ainda — falta tudo de Security + Flyway.)

## Dica

- `@PostConstruct` é `jakarta.annotation.PostConstruct` (não `javax`).
- `AuditorAware<String>` é `org.springframework.data.domain.AuditorAware`.
- O bean retornar `Optional.empty()` impede a JPA de gravar `criado_por`. Sempre retorne algum valor.

## Commit sugerido

```
feat(config): locale pt-BR + timezone America/Sao_Paulo + jpa auditing

AuditorAware stub retorna 'system' enquanto auth nao esta integrada.
Sprint 2 substitui pelo principal real.
```

## Code Review — o que vou olhar

- Locale + Timezone via `@PostConstruct` (não `static` block)?
- `AuditorAware<String>` retorna sempre um Optional populado?
- Sem `null` em retorno?
- Imports `jakarta.*` (não `javax.*`)?
