# Task 0.3 — Estrutura de Pacotes Hexagonal

## Epic / Sprint

EPIC 0 · Sprint 0

## Objetivo

Materializar a árvore de pacotes da arquitetura hexagonal antes de começar a escrever código. Disciplina visual.

## Critério de aceite

- [x] Diretórios criados com `package-info.java` em cada pacote relevante (ou `.gitkeep` se preferir manter clean).
- [x] Estrutura segue exatamente o desenho abaixo.
- [x] `./mvnw compile` continua verde (sem código novo, só pacotes).

## Estrutura

```
src/main/java/br/com/mauricio/contas/
├── ContasApiApplication.java
├── config/
├── domain/
│   ├── shared/
│   │   ├── exception/
│   │   └── command/
│   ├── usuario/
│   │   └── port/
│   │       ├── in/
│   │       └── out/
│   ├── conta/
│   │   ├── event/
│   │   └── port/
│   │       ├── in/
│   │       └── out/
│   └── transacao/
│       ├── event/
│       └── port/
│           ├── in/
│           └── out/
├── application/
│   ├── usuario/service/
│   ├── conta/service/
│   └── transacao/service/
└── infrastructure/
    ├── persistence/
    │   ├── usuario/
    │   ├── conta/
    │   └── transacao/
    ├── web/
    │   ├── usuario/dto/
    │   ├── conta/dto/
    │   ├── transacao/dto/
    │   ├── auth/
    │   └── exception/
    ├── security/
    └── event/
```

## Passos

1. Criar todos os diretórios.
2. Em cada pacote, criar `package-info.java` (opcional mas didático):
   ```java
   /**
    * Camada Domain — Usuário. ZERO dependências de Spring/JPA.
    */
   package br.com.mauricio.contas.domain.usuario;
   ```
3. `./mvnw compile`.

## Dica

- Não escreva código de domínio nesta task. Só estrutura.
- `package-info.java` é arquivo Java legal — bom p/ documentar regra arquitetural por pacote.
- Alternativa minimal: criar diretórios e cada um com `.gitkeep` vazio. Git só versiona arquivos, não pastas.

## Commit sugerido

```
chore(arch): cria estrutura de pacotes hexagonal

domain (zero deps spring/jpa), application (use case impls),
infrastructure (adapters), config (cross-cutting).
```

## Code Review — o que vou olhar

- Estrutura idêntica ao desenho?
- `domain/` realmente sem nenhum import externo?
- `package-info.java` documentando regra (ou ausência consistente)?

## Resultado do Review

**Aprovada em 2026-10-01.** O pacote `domain/shared/command` foi corrigido; estrutura e declarações dos pacotes conferem com os caminhos. `./mvnw -q compile` passou.
