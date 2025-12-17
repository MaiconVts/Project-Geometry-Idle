# Roadmap de Desenvolvimento

> **Status Global:** Fase 0 — Documentação e Planejamento  
> **Início do Desenvolvimento:** A definir  
> **Previsão de Conclusão:** 8-10 semanas após início

Este documento define os marcos de entrega (Milestones) do **Project Geometry Idle**. O foco é desenvolver de forma iterativa: primeiro a mecânica (Core), depois a estrutura (Architecture) e por fim o conteúdo (Meta-game).

---

## Legenda de Status

| Símbolo | Status       | Descrição                                      |
|---------|--------------|------------------------------------------------|
| [ ]     | A Fazer      | Planejado, mas não iniciado                    |
| [🚧]    | Em Andamento | Sendo desenvolvido atualmente                  |
| [✓]     | Concluído    | Testado e mergeado na branch `main`            |
| [🛑]    | Bloqueado    | Depende de outra feature para continuar        |

---

## Fase 0: Planejamento (Atual)

*Foco: Documentação, arquitetura e setup do projeto.*

**Duração estimada:** 1 semana

### Documentação

- [✓] Estrutura de pastas `/docs`
- [✓] Game Pitch (01-Game_Pitch.md)
- [✓] GDD (02-GDD_Game_Design_Document.md)
- [✓] Economy & Math (03-Economy_And_Math.md)
- [✓] TDD Architecture (01-TDD_Architecture.md)
- [✓] Data Persistence & Security (02-Data_Persistence_Security.md)
- [✓] Code Style Guide (03-Code_Style_Guide.md)
- [✓] UI/UX Flow (01-UI_UX_Flow.md)
- [✓] Art Bible - Estrutura (02-Art_Bible.md)
- [✓] QA & Performance Plan (01-QA_Performance_Plan.md)
- [✓] Roadmap (02-Roadmap_do_Projeto.md)
- [✓] DevLog Template (03-DevLog_e_PostMortem.md)
- [✓] Changelog (CHANGELOG.md)
- [✓] README.md

### Setup Técnico

- [ ] Criar repositório Git
- [ ] Configurar .gitignore para Unity
- [ ] Configurar .gitattributes (LFS)
- [ ] Criar projeto Unity 6
- [ ] Estrutura de pastas no Unity
- [ ] Importar pacotes base (UniTask)

---

## Fase 1: The Toy

*Foco: Criar algo divertido de assistir e interagir, sem economia ou menus.*

**Duração estimada:** 2 semanas

### Core Physics

- [ ] Configurar Physics2D settings
    - [ ] Gravity = 0
    - [ ] Physics Material (Bounciness = 1)
    - [ ] Collision layers e matrix
- [ ] Criar área de jogo (bounds/paredes)
- [ ] Prefab de projétil básico
    - [ ] Rigidbody2D configurado
    - [ ] Collider2D
    - [ ] Script de movimento
- [ ] Prefab de alvo básico
    - [ ] HP system simples
    - [ ] Detecção de colisão
    - [ ] Destruição ao HP = 0

### Spawn Systems

- [ ] SpawnManager para projéteis
    - [ ] Spawn em intervalo fixo
    - [ ] Direção aleatória em cone
- [ ] SpawnManager para alvos
    - [ ] Spawn quando abaixo do limite
    - [ ] Posição aleatória na área

### Validação da Fase 1

- [ ] É satisfatório assistir projéteis rebatendo?
- [ ] Alvos são destruídos corretamente?
- [ ] Performance OK com 100+ projéteis?

**Entrega:** Build jogável "The Toy" — mecânica base funcionando

---

## Fase 2: The Game

*Foco: Transformar a simulação em um jogo com progressão.*

**Duração estimada:** 2 semanas

### Sistema de Currency

- [ ] Implementar struct BigDouble
    - [ ] Operações matemáticas (+, -, ×, ÷)
    - [ ] Formatação para display (K, M, B, T, aa...)
    - [ ] Testes unitários
- [ ] CurrencyManager
    - [ ] Evento OnCurrencyChanged
    - [ ] Ganho ao destruir alvo

### UI Básica (HUD)

- [ ] Canvas setup
- [ ] Display de currency (top-left)
- [ ] Botão de pause (top-right)
- [ ] Painel de upgrades (bottom)

### Sistema de Upgrades

- [ ] UpgradeData (ScriptableObject)
    - [ ] ID, nome, descrição
    - [ ] Custo base e multiplicador
    - [ ] Efeito por nível
- [ ] UpgradeManager
    - [ ] Lógica de compra
    - [ ] Aplicação de efeitos
- [ ] Upgrades implementados:
    - [ ] Damage
    - [ ] Spawn Rate
    - [ ] Projectile Count

### Validação da Fase 2

- [ ] Jogador sente progresso ao comprar upgrades?
- [ ] Curva de custo está balanceada?
- [ ] UI é clara e não intrusiva?

**Entrega:** Build "The Game" — loop de progressão funcionando

---

## Fase 3: The Software

*Foco: Refatoração, performance e persistência de dados.*

**Duração estimada:** 2 semanas

### Object Pooling

- [ ] ObjectPool<T> genérico
- [ ] Pool para projéteis
- [ ] Pool para partículas (placeholder)
- [ ] Pool para floating text
- [ ] Validar zero Instantiate/Destroy no gameplay

### Save System

- [ ] SaveData model
- [ ] SaveManager (Singleton)
    - [ ] SaveAsync
    - [ ] LoadAsync
    - [ ] Backup automático
- [ ] SaveScheduler
    - [ ] Auto-save (2 min)
    - [ ] Transaction save (debounce)
    - [ ] OnApplicationPause/Quit

### Segurança Básica

- [ ] ObfuscatedInt struct
- [ ] ObfuscatedBigDouble struct
- [ ] ChecksumUtils
- [ ] SanityValidator (básico)

### Validação da Fase 3

- [ ] Save/Load funciona consistentemente?
- [ ] Zero GC allocs no Update?
- [ ] Performance estável em sessões longas?

**Entrega:** Build "The Software" — arquitetura profissional

---

## Fase 4: The Polish

*Foco: Sensação de jogo ("Juice"), som e retenção.*

**Duração estimada:** 2 semanas

### Game Feel

- [ ] VFX: Partículas de hit
- [ ] VFX: Partículas de destroy
- [ ] VFX: Floating damage text
- [ ] Screen shake (opcional, configurável)
- [ ] Animações de UI (open/close panels)

### Audio

- [ ] AudioManager (Singleton)
- [ ] SFX: Hit sounds (variação de pitch)
- [ ] SFX: Destroy sounds
- [ ] SFX: Purchase sound
- [ ] SFX: UI sounds
- [ ] Music: Background track (placeholder ou royalty-free)

### Quality of Life

- [ ] Offline gains
    - [ ] Cálculo de tempo ausente
    - [ ] Welcome back popup
- [ ] Settings panel
    - [ ] Volume sliders
    - [ ] Qualidade de partículas
- [ ] Pause menu funcional

### Dev Mode

- [ ] Input secreto para ativar
- [ ] Painel de debug
    - [ ] Adicionar currency
    - [ ] Resetar save
    - [ ] Desbloquear tudo
- [ ] Proteção contra acesso acidental

### Validação da Fase 4

- [ ] Jogo "sente" bem? Feedback satisfatório?
- [ ] Áudio não é irritante após 30 min?
- [ ] Offline gains incentivam retorno?

**Entrega:** Build "Polished" — pronto para testes externos

---

## Fase 5: Launch Prep

*Foco: Builds finais, testes e distribuição.*

**Duração estimada:** 1-2 semanas

### Integração de Serviços

- [ ] Unity Analytics
    - [ ] Eventos de gameplay
    - [ ] Funil de progressão
- [ ] Unity Cloud Diagnostics
    - [ ] Crash reporting
- [ ] Unity Remote Config
    - [ ] Variáveis de balanceamento

### Build & Distribution

- [ ] Build settings otimizados
- [ ] Ícones e splash screen (placeholder OK)
- [ ] Build para Windows
- [ ] (Futuro) Build para Android

### QA Final

- [ ] Smoke tests completos
- [ ] Stress test (4h+ de gameplay)
- [ ] Teste em hardware mínimo
- [ ] Regression test de todas features

### Validação da Fase 5

- [ ] Build inicia em todas plataformas alvo?
- [ ] Analytics recebendo dados?
- [ ] Nenhum bug crítico conhecido?

**Entrega:** Release Candidate

---

## Backlog (Futuro)

Features planejadas mas não priorizadas para v1.0:

### Meta-Game

- [ ] Sistema de Prestige/Rebirth
- [ ] Múltiplos tipos de projétil
- [ ] Múltiplos tipos de alvo
- [ ] Boss stages
- [ ] Achievements

### Conteúdo

- [ ] Definição de tema visual
- [ ] Arte final (sprites, UI)
- [ ] Música original
- [ ] Mais upgrades

### Plataformas

- [ ] Port para Mobile (Android)
- [ ] Port para iOS
- [ ] Steam integration
- [ ] Cloud save sync

### Monetização (se aplicável)

- [ ] Modelo de monetização definido
- [ ] IAP integration
- [ ] Rewarded ads (opcional)

---

## Timeline Visual

```
Semana  1    2    3    4    5    6    7    8    9   10
        |----|----|----|----|----|----|----|----|----|----|
Fase 0  [==]
Fase 1       [========]
Fase 2                 [========]
Fase 3                           [========]
Fase 4                                     [========]
Fase 5                                               [====]
```

---

## Critérios de Conclusão de Fase

Cada fase só é considerada concluída quando:

1. ✅ Todas as tasks marcadas como concluídas
2. ✅ Build estável (sem crashes conhecidos)
3. ✅ Validação da fase aprovada
4. ✅ Código mergeado na `main`
5. ✅ Documentação atualizada (se necessário)
6. ✅ Entry no DevLog/Changelog

---

## Notas

- Timeline é estimativa, não compromisso
- Prioridades podem mudar baseado em playtesting
- Fases podem se sobrepor levemente
- "Concluído" significa funcional, não perfeito