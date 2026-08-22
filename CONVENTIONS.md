# Convenções — Contas API

## Java

- **Versão**: 21 (LTS). Usa records pattern matching, sealed types, switch expressions.
- **Encoding**: UTF-8.
- **Indent**: 4 espaços.
- **Line length**: até 120.

## Naming

| Tipo | Convenção | Exemplo |
|---|---|---|
| Aggregate / Entity domain | substantivo PT-BR (singular) | `Usuario`, `Conta`, `Transacao` |
| VO record | substantivo PT-BR | `Email`, `Senha`, `Money`, `NumeroConta` |
| Use case interface (port.in) | verbo + substantivo + `UseCase` | `CriarUsuarioUseCase` |
| Use case impl (application) | mesmo nome + `Service` | `CriarUsuarioService` |
| Command record | verbo + substantivo + `Command` | `CriarUsuarioCommand` |
| Repository port (port.out) | substantivo + `RepositoryPort` | `UsuarioRepositoryPort` |
| Persistence adapter | substantivo + `PersistenceAdapter` | `UsuarioPersistenceAdapter` |
| JPA entity | substantivo + `JpaEntity` | `UsuarioJpaEntity` |
| Spring Data repo | substantivo + `JpaRepository` | `UsuarioJpaRepository` |
| Controller | substantivo plural + `Controller` | `UsuariosController` |
| DTO request | verbo/contexto + substantivo + `Request` | `CriarUsuarioRequest` |
| DTO response | substantivo + `Response` | `UsuarioResponse` |
| Web mapper | substantivo + `WebMapper` | `UsuarioWebMapper` |
| Persistence mapper | substantivo + `PersistenceMapper` | `UsuarioPersistenceMapper` |
| Domain event | substantivo + verbo passado + `Event` | `ContaAbertaEvent` |
| Exception | substantivo + verbo + `Exception` | `EmailJaCadastradoException` |

## TDD

- Teste unitário: `*Test.java` (Surefire).
- Teste integração: `*IT.java` (Failsafe + Testcontainers).
- `@DisplayName` sempre em PT-BR, descritivo.
- Padrão **Given / When / Then** com comentários no método se ajudar.
- AssertJ p/ assertions (`assertThat`, `assertThatThrownBy`).
- 1 cenário = 1 método de teste.

## Records

| Onde | Regra |
|---|---|
| DTOs Request/Response | ✅ obrigatório |
| Value Objects | ✅ obrigatório |
| Commands | ✅ obrigatório |
| Domain Events | ✅ obrigatório |
| Query Results / Views | ✅ obrigatório |
| Aggregate roots | ⚠️ classe POJO (estado evolui) OU sealed + records p/ hierarquia |
| JPA entities | ❌ proibido (Hibernate exige no-args ctor + setters) |
| Exceptions | ❌ proibido |

## Lombok

- ✅ permitido em: JPA entities, classes mutáveis de adapter, exceptions.
- ❌ proibido em: records, value objects, controllers.

## Hexagonal

- `domain/` **ZERO** imports de Spring/JPA/Jackson. Permitido: `java.*`, `jakarta.validation.*`.
- `application/` pode importar Spring (`@Service`, `@Transactional`). NÃO importa Jackson nem JPA.
- `infrastructure/` é a única camada que pode importar Spring Web, JPA, Jackson.

## Commits

Formato Conventional Commits:

| Tipo | Quando |
|---|---|
| `test: red — <desc>` | escreveu teste vermelho |
| `feat: green — <desc>` | implementou código que passa o teste |
| `refactor: <desc>` | refatoração sem mudar comportamento |
| `chore: <desc>` | infra, deps, configs |
| `docs: <desc>` | docs ou comentários |
| `fix: <desc>` | bugfix com teste regressão |

## Branch

- `main` — protegida, merge via PR.
- `sprint-X/task-Y.Z-slug` — branch por task.

(opcional, vc decide se quer disciplina de PR ou commit direto em main p/ estudo)
