# Art Bible (Style Guide)

## Propósito deste Documento

Define a direção artística do jogo. Mesmo sendo formas geométricas, é preciso consistência visual. Este documento guia a estética.

> ⚠️ **STATUS: PENDENTE DE DEFINIÇÃO**
> 
> O estilo visual do jogo ainda não foi decidido. Este documento contém a estrutura que será preenchida após a escolha do tema. O desenvolvimento atual usa **placeholders geométricos** (greyboxing).

---

## Decisões Pendentes

### Tema Visual

| Opção              | Descrição                                    | Mood               |
|--------------------|----------------------------------------------|--------------------|
| **Minimalista**    | Formas puras, sem textura, cores sólidas     | Clean, relaxante   |
| **Neon/Cyberpunk** | Glow intenso, cores vibrantes, grid digital  | Energético, tech   |
| **Medieval**       | Setas, espadas, criaturas fantasy            | Épico, aventura    |
| **Sci-fi**         | Lasers, naves, asteroides                    | Futurista, espacial|
| **Natureza**       | Elementos orgânicos, plantas, animais        | Calmo, orgânico    |

### Entidades do Jogo

| Elemento Genérico | Opções em Consideração                          |
|-------------------|-------------------------------------------------|
| Projectile        | Seta, laser, shuriken, magia, bomba             |
| Target            | Forma geométrica, criatura, estrutura, mineral  |
| Currency          | Ouro, cristais, energia, polígonos, moedas      |
| Background        | Abstrato, cenário temático, gradiente           |

---

## Estrutura do Art Bible (A ser preenchido)

### Paleta de Cores

```
Quando definido, incluir:
- Cor primária (HEX)
- Cor secundária (HEX)
- Cor de destaque (HEX)
- Cor de fundo (HEX)
- Cor de UI (HEX)
- Cor de texto (HEX)
- Cor de sucesso (HEX)
- Cor de erro (HEX)
```

**Placeholder atual (Greyboxing):**

| Elemento    | Cor         | Hex       |
|-------------|-------------|-----------|
| Background  | Cinza escuro| `#1a1a2e` |
| Projectile  | Branco      | `#ffffff` |
| Target      | Cinza médio | `#4a4a6a` |
| UI Text     | Branco      | `#ffffff` |
| UI Accent   | Ciano       | `#00d4ff` |

### Tipografia

```
Quando definido, incluir:
- Fonte de títulos (nome, peso, tamanho base)
- Fonte de corpo (nome, peso, tamanho base)
- Fonte de números (se diferente)
- Hierarquia de tamanhos (H1, H2, body, small)
```

**Placeholder atual:**

| Uso         | Fonte              | Tamanho Base |
|-------------|--------------------|--------------|
| Títulos     | Sistema (Bold)     | 32px         |
| Corpo       | Sistema (Regular)  | 16px         |
| Números     | Sistema (Mono)     | 24px         |

### Estilo de Ícones

```
Quando definido, incluir:
- Estilo (outline, filled, duotone)
- Espessura de linha
- Raio de borda
- Biblioteca de referência ou custom
```

---

## Especificações Técnicas (Fixas)

Independente do tema escolhido, essas especificações são constantes:

### Resolução de Assets

| Tipo           | Resolução Base | Variantes           |
|----------------|----------------|---------------------|
| Sprites        | 64x64 px       | @1x, @2x, @4x       |
| UI Icons       | 48x48 px       | @1x, @2x            |
| Backgrounds    | 1920x1080 px   | Tiled ou stretched  |
| Particles      | 32x32 px       | @1x                 |

### Formato de Arquivos

| Tipo           | Formato    | Compressão         |
|----------------|------------|--------------------|
| Sprites        | PNG        | Crunch (Unity)     |
| UI             | PNG        | Sem compressão     |
| Backgrounds    | PNG/JPG    | Depende do uso     |
| Animações      | Sprite Sheet| PNG               |

### Layers de Renderização

```
Sorting Layers (back to front):
1. Background        (z: -100)
2. Targets           (z: -50)
3. Projectiles       (z: 0)
4. Effects           (z: 50)
5. UI                (z: 100)
```

---

## VFX (Efeitos Visuais)

### Partículas Planejadas

| Efeito              | Trigger                  | Duração | Notas               |
|---------------------|--------------------------|---------|---------------------|
| Hit Impact          | Projétil atinge alvo     | 0.2s    | Pequeno, direcional |
| Destroy Burst       | Alvo destruído           | 0.5s    | Explosivo, radial   |
| Currency Collect    | Currency coletado        | 0.3s    | Partículas sobem    |
| Level Up            | Upgrade comprado         | 1.0s    | Celebratório        |
| Critical Hit        | Dano crítico             | 0.3s    | Maior que hit normal|

### Shaders Planejados

| Shader          | Uso                         | Prioridade |
|-----------------|-----------------------------|------------|
| Sprite Default  | Maioria dos sprites         | Core       |
| Glow/Emission   | Projéteis, UI highlights    | Nice-to-have|
| Dissolve        | Alvos sendo destruídos      | Nice-to-have|
| Screen Distortion| Impactos fortes            | Polish     |

---

## Referências Visuais

> 📌 **Seção para adicionar:**
> - Screenshots de jogos inspiradores
> - Mood boards
> - Concept art
> - Paletas de cor de referência

### Jogos de Referência

| Jogo            | Elemento de Inspiração                |
|-----------------|---------------------------------------|
| Zen Shards      | Destruição satisfatória, minimalismo  |
| Geometry Wars   | Neon, partículas, gameplay frenético  |
| Cookie Clicker  | UI de idle, feedback de números       |
| Alto's Odyssey  | Paleta de cores, atmosfera zen        |

---

## Greyboxing Atual

Durante a fase de prototipagem, todos os elementos usam formas geométricas básicas:

```
Projectile:  ▷ (triângulo apontando direção)
Target:      ■ (quadrado) ou ● (círculo)
Currency:    ◆ (losango)
Background:  Cor sólida ou gradiente simples
```

### Benefícios do Greyboxing

1. **Foco na mecânica:** Testa se o core loop é divertido sem distração visual
2. **Iteração rápida:** Mudanças não requerem arte nova
3. **Flexibilidade:** Qualquer tema pode ser aplicado depois
4. **Performance:** Assets simples = fácil otimização

### Critérios para Sair do Greyboxing

- [ ] Core loop validado como "divertido"
- [ ] Sistemas de upgrade funcionando
- [ ] Performance estável (60 FPS com 500+ entidades)
- [ ] Decisão de tema aprovada
- [ ] Budget de arte definido (assets próprios vs. comprados)

---

## Checklist de Arte (Para quando definido)

### Assets Necessários

**Sprites:**
- [ ] Projétil (+ variantes se houver tipos)
- [ ] Alvo (+ variantes de HP/tipo)
- [ ] Ícones de upgrade (damage, speed, count, crit, etc)
- [ ] Ícone de currency
- [ ] Botões de UI (normal, hover, pressed, disabled)
- [ ] Backgrounds (+ variantes)

**Partículas:**
- [ ] Hit impact
- [ ] Destroy burst
- [ ] Currency collect
- [ ] Trail do projétil

**UI:**
- [ ] Logo do jogo
- [ ] Painel de upgrade
- [ ] Painel de pause
- [ ] Barras de progresso
- [ ] Sliders de volume

**Áudio (referência cruzada):**
- [ ] Hit sounds
- [ ] Destroy sounds
- [ ] UI sounds
- [ ] Background music

---

## Notas de Implementação

### Sprite Atlas

Todos os sprites devem ser empacotados em Sprite Atlas para reduzir draw calls:

```
Atlas/
├── UI_Atlas.spriteatlas       (todos elementos de UI)
├── Gameplay_Atlas.spriteatlas (projéteis, alvos, efeitos)
└── Particles_Atlas.spriteatlas (texturas de partícula)
```

### Animation Clips

Se houver animações de sprite:

```
Animations/
├── Projectile/
│   └── Projectile_Idle.anim
├── Target/
│   ├── Target_Idle.anim
│   ├── Target_Hit.anim
│   └── Target_Destroy.anim
└── UI/
    └── Button_Press.anim
```