# Checklist de Code Review

Aplicado em todo código que você entregar. Aprovação requer **todos** os itens marcados.

## TDD

- [ ] Existe commit `test: red` ANTES do `feat: green` no histórico?
- [ ] Teste cobre cenário positivo + negativo + edge cases?
- [ ] Teste é independente (não depende de ordem)?
- [ ] `@DisplayName` em PT-BR descritivo?

## Arquitetura Hexagonal

- [ ] `domain/` não importa nada de `org.springframework.*`, `jakarta.persistence.*`, `com.fasterxml.*`?
- [ ] Port (interface) está em `domain/.../port/in` ou `domain/.../port/out`?
- [ ] Use case impl está em `application/`?
- [ ] Adapter está em `infrastructure/`?

## SOLID

- [ ] **S** Classe tem 1 responsabilidade clara?
- [ ] **O** Extensível sem modificar código existente onde faz sentido?
- [ ] **L** Subtipos respeitam contrato do supertipo?
- [ ] **I** Interfaces pequenas e específicas (não god-interface)?
- [ ] **D** Depende de abstração (port), não impl concreta?

## Records

- [ ] DTO é record?
- [ ] VO é record?
- [ ] Command é record?
- [ ] Compact constructor valida invariantes?
- [ ] NÃO usa Lombok em records?

## Naming

- [ ] Segue `CONVENTIONS.md`?
- [ ] Sem abreviação obscura (`usr` → `usuario`)?
- [ ] Métodos no infinitivo (`criar`, `buscar`, `validar`)?

## Validação & Erros

- [ ] Bean Validation (`@NotBlank`, `@Size`) nos DTOs?
- [ ] Exception customizada estende `DomainException` (não `RuntimeException` solta)?
- [ ] `ApiExceptionHandler` mapeia exception → HTTP status correto?
- [ ] Mensagem de erro útil ao consumidor?

## Clean Code

- [ ] Método < 20 linhas (preferência)?
- [ ] Classe < 200 linhas?
- [ ] Sem `null` retornado de público (usa `Optional` ou exception)?
- [ ] Sem `// TODO` sem issue vinculada?
- [ ] Sem código morto / import não usado?
- [ ] Sem comentário óbvio (`// soma a + b`)?

## Testes verde + cobertura

- [ ] `./mvnw verify` verde?
- [ ] Cobertura ≥ 80% nas classes alteradas?

---

## Como entregar p/ review

Cole no chat (formato `markdown` com code blocks):

```
## Task X.Y — <subject>

### Commits
<lista hash + msg>

### Arquivos
<lista paths>

### Código novo / alterado
```java
// path/to/File.java
...
```

### Saída do `./mvnw verify`
<copiar resumo>
```

Eu reviso linha-a-linha. Resposta: 🟢 aprovado / 🟡 ajustes / 🔴 reprovado.
