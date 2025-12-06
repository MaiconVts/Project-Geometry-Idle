# Project Geometry Idle

> **Status:** Em Desenvolvimento (Fase de Planejamento/Arquitetura)  
> **Plataforma Alvo:** PC (Steam) - *Port Mobile Futuro* > **Engine:** Unity 6 (LTS)  
> **Linguagem:** C# (.NET Standard 2.1)

---

## 1. Visão Geral do Projeto
**Project Geometry Idle** é uma simulação de física baseada em "Idle/Clicker Game", inspirada na estética generativa e mecânicas de *Zen Shards*. O objetivo é criar um sistema autônomo de destruição de formas geométricas processuais, focando em satisfação visual e progressão numérica exponencial.

O diferencial técnico do projeto reside na **Engenharia de Performance** (gerenciamento de memória e física para milhares de entidades) e **Arquitetura de Dados Segura** (prevenção de cheat e persistência assíncrona).

---

## 2. Estimativa de Projeto
Considerando uma rotina de desenvolvimento de **5 dias por semana, 8 horas por dia** (com curva de aprendizado inclusa):

* **Esforço Total Estimado:** ~320 a 400 horas.
* **Duração de Calendário:** 8 a 10 semanas.
* **Fases Críticas:**
    * *Core Loop & Física:* 2 Semanas.
    * *Arquitetura de Sistemas (Save/Events):* 2 Semanas.
    * *Game Design & UI:* 3 Semanas.
    * *Polimento & Ferramentas:* 2 Semanas.

---

## 3. Tech Stack e Ferramentas

### Ambiente de Desenvolvimento
* **Engine:** Unity 6 (LTS) ou 2022.3 LTS (Foco em estabilidade).
* **IDE:** Visual Studio 2022 Community (Intellisense avançado para C#).
* **Versionamento:** Git + GitHub (Com `.gitignore` específico para Unity).
* **Backend Services:** Unity Gaming Services (UGS).

### Bibliotecas e Frameworks Planejados
* **Física:** Box2D (Nativo Unity 2D) otimizado.
* **Async:** `UniTask` ou `Tasks` nativas do .NET para operações de I/O não bloqueantes.
* **Matemática:** Estrutura customizada `BigDouble` (Struct) para lidar com números superiores a $10^{308}$.
* **Serialização:** `JsonUtility` (Alta performance/Baixa alocação).
* **Analytics:** Unity Analytics.
* **Remote Config:** Unity Remote Config (Ajustes de balanceamento em tempo real).

---

## 4. Arquitetura de Software

O projeto segue princípios de **SOLID** e **Clean Code**, priorizando desacoplamento e performance.

### 4.1. Padrões de Projeto (Design Patterns)
* **Observer Pattern (Event-Driven):** Utilização de `C# Actions/Events` para comunicação entre sistemas. A lógica do jogo (Model) não conhece a UI (View).  
  * *Ex:* O `Bloco` emite `OnDestroyed`. O `ScoreManager`, `AudioManager` e `VFXManager` escutam e reagem independentemente.
* **Object Pooling:** Reutilização agressiva de objetos (Setas, Partículas, Texto de Dano) para evitar alocação de memória e picos de Garbage Collection (GC).
* **Singleton (Gerenciadores):** Acesso centralizado para `GameManager`, `SaveManager` e `AudioManager`.
* **Strategy Pattern:** Para comportamentos de upgrades (ex: diferentes lógicas de cálculo de dano ou movimento).

### 4.2. Estrutura de Dados
* **Big Numbers:** Uso de `Structs` (Value Types) ao invés de Classes para operações matemáticas frequentes, minimizando pressão no Heap de memória.
* **ScriptableObjects:** Utilizados como "Banco de Dados Estático" para configuração de balanceamento (atributos base das setas, custos de upgrade, tabelas de loot).

---

## 5. Persistência de Dados e I/O

O sistema de salvamento foca na integridade dos dados e fluidez da gameplay (evitando "stuttering").

### 5.1. Estratégia Híbrida de Save
1. **Interceptor de Transação:** Monitora eventos críticos (compras, desbloqueios). Ao ocorrerem, enfileira um save imediato.
2. **Debounce Logic (Amortecimento):** O sistema possui um `Cooldown` (ex: 60s). Se múltiplas transações ocorrem em segundos, apenas uma operação de disco é realizada ao final do timer.
3. **Auto-Save Periódico:** Trigger por tempo (a cada 2 min) para backup de progresso passivo (Gold/Tempo).
4. **Save on Exit/Pause:** Garante persistência ao fechar o aplicativo.

### 5.2. Async I/O
* Toda escrita no disco (`File.WriteAllText`) é executada em uma **Thread Secundária** via `Task.Run`, garantindo que a Thread Principal (Rendering/Physics) permaneça a 60 FPS estáveis.

---

## 6. Segurança e Integridade (Anti-Cheat)

### 6.1. Ofuscação de Memória
* Variáveis sensíveis (Gold, Premium Currency, Dano) não são armazenadas como `int` ou `double` puros.
* Utilização de uma `Struct` encapsulada que aplica uma operação **XOR** com uma chave aleatória no valor armazenado, dificultando a localização via *Memory Scanners* (ex: Cheat Engine).

### 6.2. Sanity Checks e Soft Ban
* **Validação:** Ao carregar ou realizar compras, o sistema verifica se os valores são matematicamente possíveis (ex: Ouro ganho vs. Tempo jogado).
* **Penalty System (Soft Ban):** Em caso de detecção de anomalia, o jogador não é banido. O sistema aplica um "Resfriamento de Compras":
    * *Infração 1:* Bloqueio de 1 minuto.
    * *Infração 2:* Bloqueio de 10 minutos.
    * *Infração 3:* Bloqueio de 60 minutos.
    * *Mensagem In-Game:* "Sistema instável. Aguarde resfriamento."

### 6.3. Dev Mode (God Mode)
* Acesso restrito a um painel de debug (Adicionar Ouro, Resetar Save, Desbloquear Tudo).
* **Acesso:** Combinação de inputs invisíveis na UI (Máquina de Estados de Cliques: 10x Área A -> 5x Área B -> 5x Área C).
* **Autenticação:** Input de senha com proteção contra Brute Force (3 erros = bloqueio temporário do input).
* O código do Dev Mode é isolado via diretivas `#if DEVELOPMENT_BUILD` ou lógica interna segura.

---

## 7. DevOps e Monitoramento

* **Unity Remote Config:** Variáveis de balanceamento (Dano base, Preço base, Dificuldade) hospedadas na nuvem. Permite ajustes de gameplay sem necessidade de atualizar o binário na loja.
* **Cloud Diagnostics (Crashlytics):** Envio automático de Stack Traces em caso de Exceções Fatais ou Crash do aplicativo.

---

## 8. Estrutura de Pastas Sugerida

```text
Assets/
├── _Project/                 # Todo o conteúdo autoral do projeto
│   ├── Animations/
│   ├── Audio/
│   ├── Data/                 # ScriptableObjects (Configurações)
│   ├── Materials/            # Materiais Físicos e Visuais
│   ├── Prefabs/
│   │   ├── Core/             # Setas, Blocos
│   │   ├── UI/               # Menus, Popups
│   │   └── Systems/          # Managers (Não visuais)
│   ├── Scenes/
│   ├── Scripts/
│   │   ├── Core/             # Lógica principal (Game Loop)
│   │   ├── Systems/          # Save, Audio, Pooling
│   │   ├── Controllers/      # Input, Camera
│   │   ├── UI/               # View Logic
│   │   ├── Data/             # Modelos de Dados (Structs, BigDouble)
│   │   ├── Utils/            # Helpers, Extensions
│   │   └── Security/         # Anti-cheat, Obfuscation
│   ├── Sprites/              # Assets 2D (Greyboxing inicial)
│   └── Tests/                # PlayMode e EditMode Tests
├── Plugins/                  # SDKs de terceiros (Unity Analytics, etc)
└── Settings/                 # Configurações de Render Pipeline
