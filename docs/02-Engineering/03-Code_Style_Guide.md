# Code Style Guide & Git Standards

## Propósito deste Documento

Define as regras de escrita de código e versionamento para manter o projeto limpo, legível e profissional. Essencial para manutenção a longo prazo e padronização da equipe.

---

## Padrões de Git & Commit

Para manter o histórico do projeto legível e rastreável, todos os commits devem seguir estritamente o padrão de **Semantic Commits**.

### Estrutura da Mensagem
```text
tipo(escopo) - Descrição curta e objetiva
- Detalhe adicional do que foi feito (opcional)
- Outro detalhe relevante (opcional)
```

### Tipos Permitidos

| Tipo       | Descrição                                                              |
|------------|------------------------------------------------------------------------|
| `feat`     | Nova funcionalidade (ex: nova mecânica, novo menu)                     |
| `fix`      | Correção de bug                                                        |
| `docs`     | Alterações apenas em documentação (MD, comentários)                    |
| `style`    | Formatação, ponto e vírgula, indentação (não muda lógica)              |
| `refactor` | Alteração de código que não muda funcionalidade nem corrige bug        |
| `perf`     | Mudança de código para melhorar performance                            |
| `test`     | Adição ou correção de testes                                           |
| `chore`    | Alterações em build, configs, ferramentas, bibliotecas, setup inicial  |

### Exemplos Práticos

**Feature:**
```text
feat(physics) - Adiciona sistema de rebote da seta
- Configura PhysicsMaterial2D
- Ajusta gravidade para zero
```

**Setup:**
```text
chore(setup) - Cria estrutura inicial de pastas e docs
```

---

## Padrões de Código (C# & Unity)

### Nomenclatura

| Elemento                          | Convenção       | Exemplo                                    |
|-----------------------------------|-----------------|-------------------------------------------|
| Classes / Métodos / Enums         | `PascalCase`    | `public class ArrowController { }`        |
| Interfaces                        | `IPascalCase`   | `public interface IDamageable { }`        |
| Variáveis Locais / Parâmetros     | `camelCase`     | `int currentDamage = 10;`                 |
| Campos Privados                   | `_camelCase`    | `private float _spawnRate;`               |
| Propriedades Públicas             | `PascalCase`    | `public int Health { get; private set; }` |
| Constantes                        | `UPPER_CASE`    | `private const int MAX_ARROWS = 500;`     |

### Estrutura de Arquivos

Organize scripts em pastas pelo que eles fazem (**Feature**), não pelo tipo técnico.
```text
✅ Scripts/Features/Arrows/ArrowController.cs
✅ Scripts/Features/UI/ScoreDisplay.cs
❌ Scripts/Controllers/ArrowController.cs
```

---

## Boas Práticas Unity (Performance Crítica)

### 1. Evite `Find` e `GetComponent` no Update

Operações pesadas. Faça cache das referências no `Awake` ou `Start`.
```csharp
// ❌ Ruim
void Update() {
    GetComponent<Rigidbody2D>().velocity = Vector2.up;
}

// ✅ Bom
private Rigidbody2D _rb;

void Awake() {
    _rb = GetComponent<Rigidbody2D>();
}

void Update() {
    _rb.velocity = Vector2.up;
}
```

### 2. Use `CompareTag`
```csharp
// ❌ Ruim - aloca string
if (collider.tag == "Player")

// ✅ Bom - sem alocação
if (collider.CompareTag("Player"))
```

### 3. Evite LINQ em loops frequentes

`.Where`, `.Select`, `.ToList` alocam memória. Evite no `Update()`.

### 4. Cuidado com Strings

Evite concatenação (`"Score: " + score`) no `Update()`. Use `StringBuilder` ou cache.

### 5. Coroutines vs Async

Prefira `UniTask` ou `async/await` para lógica. Use Coroutines apenas para animações visuais simples.

---

## Documentação de Código

Utilize comentários XML para métodos públicos, classes e interfaces.
```csharp
/// <summary>
/// Calcula o dano total baseado nos upgrades atuais e multiplicadores globais.
/// </summary>
/// <param name="baseDamage">O dano base da seta nível 1.</param>
/// <returns>Valor final do dano em BigDouble.</returns>
public BigDouble CalculateFinalDamage(BigDouble baseDamage) 
{
    // Lógica aqui
}
```