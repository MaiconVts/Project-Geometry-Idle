# Game Pitch & Concept

## Propósito deste Documento

Este arquivo serve como a "visão executiva" do projeto. Ele deve ser lido por qualquer pessoa que queira entender **o que é o jogo** em menos de 5 minutos, sem entrar em detalhes técnicos profundos.

---

## Elevator Pitch

> **"Um idle game hipnótico onde projéteis autônomos destroem formas processuais infinitamente, combinando física satisfatória com progressão numérica exponencial."**

---

## Gênero e Plataforma

| Aspecto       | Descrição                                      |
|---------------|------------------------------------------------|
| Gênero        | Idle / Clicker Game + Simulação de Física      |
| Plataforma    | PC (Steam) - Port Mobile planejado para futuro |
| Engine        | Unity 6 (LTS)                                  |
| Perspectiva   | 2D                                             |

---

## USP (Unique Selling Points)

O que torna **Project Geometry Idle** único:

1. **Performance Massiva:** Arquitetura otimizada para milhares de entidades simultâneas sem quedas de FPS, usando Object Pooling agressivo e gerenciamento de memória com Structs.

2. **Satisfação Visual:** Destruição procedural com feedback visual e sonoro imediato — o jogador sente cada impacto.

3. **Progressão Exponencial:** Sistema de números grandes (BigDouble) que permite escalar infinitamente, mantendo o senso de progresso constante.

4. **Sistema Autônomo:** O jogo "joga sozinho" de forma satisfatória, permitindo progressão passiva enquanto o jogador assiste ou faz outras coisas.

5. **Arquitetura Anti-Cheat:** Proteção de memória via XOR e validação de sanidade dos dados, sem punir jogadores legítimos com DRM invasivo.

---

## Público-Alvo

| Perfil                  | Descrição                                                        |
|-------------------------|------------------------------------------------------------------|
| **Primário**            | Fãs de idle/clicker games que buscam satisfação visual passiva   |
| **Secundário**          | Jogadores casuais que querem relaxar com gameplay não-exigente   |
| **Terciário**           | Entusiastas de simulação de física e destruição procedural       |

**Comportamento típico:**

- Joga em segundo plano enquanto trabalha ou assiste conteúdo
- Gosta de ver números crescendo exponencialmente
- Aprecia feedback visual e sonoro satisfatório
- Prefere progressão constante sem necessidade de habilidade mecânica

---

## Referências e Inspirações

### Jogos Similares

| Jogo             | Elemento Inspirador                                    |
|------------------|--------------------------------------------------------|
| **Zen Shards**   | Estética generativa, destruição satisfatória, física   |
| **Cookie Clicker** | Progressão numérica exponencial, upgrades em camadas |
| **Idle Breakout**  | Combinação de física com mecânicas idle              |

### Inspirações Visuais

> ⚠️ **Nota:** O estilo visual ainda não foi definido. As opções em consideração incluem:
> - Minimalista geométrico (formas puras, sem textura)
> - Neon cyberpunk (glow intenso, cores vibrantes)
> - Medieval/Fantasy (setas, espadas, criaturas)
> - Sci-fi (lasers, naves, asteroides)

---

## Core Loop Resumido

```
[Projéteis disparam automaticamente]
         ↓
[Alvos são destruídos]
         ↓
[Jogador ganha Currency passivamente]
         ↓
[Currency compra upgrades (mais projéteis, mais dano)]
         ↓
[Loop reinicia com maior escala]
```

---

## Decisões de Design Pendentes

| Elemento          | Status      | Opções em Consideração                      |
|-------------------|-------------|---------------------------------------------|
| Tipo de Projétil  | ❓ Indefinido | Setas, bombas, shurikens, lasers, magias   |
| Tipo de Alvo      | ❓ Indefinido | Formas geométricas, criaturas, estruturas  |
| Moeda do Jogo     | ❓ Indefinido | Ouro, cristais, energia, polígonos         |
| Tema Visual       | ❓ Indefinido | Minimalista, neon, medieval, sci-fi        |
| Nome Final        | ❓ Indefinido | "Geometry Idle" é placeholder              |

Essas decisões serão tomadas após validação do core loop com placeholders.