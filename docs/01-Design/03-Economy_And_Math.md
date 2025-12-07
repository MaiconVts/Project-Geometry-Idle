# Economy & Math Balance

## Propósito deste Documento

Documento focado exclusivamente na matemática do jogo. Em jogos Idle, o balanceamento é tudo. Aqui definimos as curvas de crescimento para evitar que o jogo fique fácil demais ou impossível.

> ⚠️ **Nota:** Valores são placeholders para teste. Balanceamento final requer playtesting extensivo.

---

## Princípios de Design Econômico

### Filosofia

1. **Sempre há algo para comprar:** O jogador nunca deve ficar "esperando" por muito tempo
2. **Progresso constante:** Mesmo pequeno, o jogador deve sentir avanço a cada sessão
3. **Decisões significativas:** Escolher entre upgrades deve ter trade-offs
4. **Curva de dificuldade suave:** Sem "walls" artificiais

### Métricas Alvo

| Métrica                    | Valor Alvo        | Justificativa                    |
|----------------------------|-------------------|----------------------------------|
| Tempo para primeiro upgrade | 5-10 segundos    | Gratificação imediata            |
| Upgrades por minuto (early) | 2-3              | Sensação de progresso rápido     |
| Upgrades por minuto (late)  | 0.5-1            | Mantém engajamento sem trivializar |
| Sessão média               | 5-15 minutos      | Casual-friendly                  |

---

## Fórmulas Core

### Custo de Upgrade

Progressão geométrica clássica de idle games:

```
Custo(n) = CustoBase × (Multiplicador ^ n)
```

Onde:
- `n` = nível atual do upgrade
- `CustoBase` = custo no nível 0
- `Multiplicador` = taxa de crescimento (tipicamente 1.10 a 1.20)

**Exemplo:**

```csharp
// Upgrade de Dano
CustoBase = 10
Multiplicador = 1.15

Nivel 0:  10 × 1.15^0  = 10
Nivel 1:  10 × 1.15^1  = 11.5
Nivel 5:  10 × 1.15^5  = 20.11
Nivel 10: 10 × 1.15^10 = 40.46
Nivel 50: 10 × 1.15^50 = 10,836.57
Nivel 100: 10 × 1.15^100 = 1,174,313.45
```

### Efeito de Upgrade

Depende do tipo de upgrade:

**Multiplicativo (Dano, Crit Damage):**
```
Efeito(n) = 1 + (EfeitoBase × n)

// Exemplo: +10% por nível
Nivel 10: 1 + (0.10 × 10) = 2.0 (200% do dano base)
Nivel 50: 1 + (0.10 × 50) = 6.0 (600% do dano base)
```

**Aditivo (Spawn Rate, Projectile Count):**
```
Efeito(n) = ValorBase + (EfeitoBase × n)

// Exemplo: Spawn Rate base 1.0s, -0.05s por nível
Nivel 10: 1.0 + (-0.05 × 10) = 0.5s entre spawns
Nivel 18: 1.0 + (-0.05 × 18) = 0.1s (cap mínimo)
```

### HP dos Alvos

Escala com o "estágio" ou progresso do jogador:

```
HP(stage) = HPBase × (EscalaHP ^ stage)
```

**Exemplo:**

```csharp
HPBase = 10
EscalaHP = 1.08

Stage 1:   10 × 1.08^1   = 10.8
Stage 10:  10 × 1.08^10  = 21.6
Stage 50:  10 × 1.08^50  = 469.0
Stage 100: 10 × 1.08^100 = 21,992.0
Stage 200: 10 × 1.08^200 = 48,366,286.0
```

### Recompensa por Alvo

Proporcional ao HP máximo (não atual) do alvo:

```
Reward = HPMax × RewardMultiplier × UpgradeBonus
```

**Exemplo:**

```csharp
HPMax = 100
RewardMultiplier = 0.1  // 10% do HP vira currency
UpgradeBonus = 1.5      // +50% de upgrade de ganho

Reward = 100 × 0.1 × 1.5 = 15 currency
```

---

## Tabelas de Balanceamento

### Upgrades - Valores Base

| Upgrade         | Custo Base | Mult. Custo | Efeito/Nível | Tipo         | Max |
|-----------------|------------|-------------|--------------|--------------|-----|
| Damage          | 10         | 1.15        | +10%         | Multiplicativo| ∞  |
| Spawn Rate      | 50         | 1.20        | -0.05s       | Aditivo      | 18  |
| Projectile Count| 500        | 1.50        | +1           | Aditivo      | 20  |
| Crit Chance     | 100        | 1.25        | +1%          | Aditivo      | 75  |
| Crit Damage     | 200        | 1.30        | +25%         | Multiplicativo| ∞  |
| Currency Mult   | 150        | 1.18        | +5%          | Multiplicativo| ∞  |
| Offline Gain    | 1000       | 1.40        | +5%          | Multiplicativo| 50 |

### Progressão de Stages

| Stage Range | HP Multiplier | Reward Multiplier | Novos Elementos       |
|-------------|---------------|-------------------|-----------------------|
| 1-10        | 1.08          | 0.10              | Tutorial implícito    |
| 11-25       | 1.10          | 0.12              | Primeiro prestige?    |
| 26-50       | 1.12          | 0.15              | Tipos de alvo variados|
| 51-100      | 1.15          | 0.18              | Boss stages?          |
| 100+        | 1.18          | 0.20              | Endgame scaling       |

---

## Sistema de Big Numbers

### Por que Big Numbers?

Idle games frequentemente ultrapassam o limite de `double` (≈1.8×10^308). Precisamos de uma estrutura que suporte números arbitrariamente grandes.

### Implementação: BigDouble

```csharp
public readonly struct BigDouble
{
    public readonly double Mantissa;  // 1.0 a 9.999...
    public readonly long Exponent;    // Potência de 10
    
    // 1.5e12 = Mantissa: 1.5, Exponent: 12
    // Representa: 1,500,000,000,000
}
```

### Formatação para Display

| Valor               | Exponent | Display        |
|---------------------|----------|----------------|
| 1,000               | 3        | 1,000          |
| 10,000              | 4        | 10.00K         |
| 1,000,000           | 6        | 1.00M          |
| 1,000,000,000       | 9        | 1.00B          |
| 1,000,000,000,000   | 12       | 1.00T          |
| 10^15               | 15       | 1.00aa         |
| 10^18               | 18       | 1.00ab         |
| 10^21               | 21       | 1.00ac         |

### Sufixos (Notação Alfabética)

```csharp
private static readonly string[] Suffixes = 
{
    "", "K", "M", "B", "T",           // 10^3, 10^6, 10^9, 10^12
    "aa", "ab", "ac", "ad", "ae",     // 10^15, 10^18, ...
    "ba", "bb", "bc", "bd", "be",
    "ca", "cb", "cc", "cd", "ce",
    // ... continua até zz
};

public string ToFormattedString()
{
    if (Exponent < 4) 
        return Value.ToString("N0");  // 1,234
    
    int suffixIndex = (int)(Exponent / 3);
    double displayValue = Mantissa * Math.Pow(10, Exponent % 3);
    
    return $"{displayValue:F2}{Suffixes[suffixIndex]}";  // 1.23M
}
```

---

## Curvas de Progressão

### Early Game (0-30 min)

**Objetivo:** Ensinar mecânicas, dar upgrades frequentes

```
Currency/min: 100 → 1,000
Upgrades comprados: ~20-30
Sensação: "Estou ficando mais forte rápido!"
```

### Mid Game (30 min - 2h)

**Objetivo:** Introduzir decisões, desacelerar levemente

```
Currency/min: 1,000 → 100,000
Upgrades comprados: ~50-80
Sensação: "Preciso escolher o que priorizar"
```

### Late Game (2h+)

**Objetivo:** Manter engajamento, teaser de prestige

```
Currency/min: 100,000 → 10M+
Upgrades comprados: ~100+
Sensação: "Os números são absurdos e eu amo isso"
```

---

## Ganhos Offline

### Fórmula

```
GanhoOffline = GanhoOnline × Eficiencia × TempoAusente × UpgradeBonus

Onde:
- GanhoOnline = Currency/segundo quando jogando
- Eficiencia = 0.25 (25% do ganho online por padrão)
- TempoAusente = min(TempoReal, CapMaximo)
- CapMaximo = 8 horas (28,800 segundos)
- UpgradeBonus = 1 + (0.05 × NivelUpgradeOffline)
```

### Exemplo

```csharp
// Jogador ganha 1000/s online
// Ficou 12 horas offline
// Tem upgrade offline nível 10

GanhoOnline = 1000
Eficiencia = 0.25
TempoAusente = min(43200, 28800) = 28800  // cap de 8h
UpgradeBonus = 1 + (0.05 × 10) = 1.5

GanhoOffline = 1000 × 0.25 × 28800 × 1.5 = 10,800,000 currency
```

---

## Simulação e Testes

### Planilha de Simulação

> 📊 **Link:** [A ser criado - Google Sheets]

A planilha deve simular:
- Progressão de 0 a 100 stages
- Tempo para atingir cada marco
- Curva de currency over time
- Comparação de estratégias de upgrade

### Métricas para Monitorar

| Métrica                 | Alerta se...                        |
|-------------------------|-------------------------------------|
| Tempo para Stage 10     | > 5 minutos                         |
| Tempo para Stage 50     | > 2 horas                           |
| Currency/hora (late)    | Estagnado por > 30 min              |
| Upgrade mais comprado   | Um único domina > 50% das compras   |

---

## Ajustes via Remote Config

Variáveis que podem ser ajustadas sem rebuild:

| Variável              | Tipo   | Default | Descrição                     |
|-----------------------|--------|---------|-------------------------------|
| `base_damage`         | float  | 1.0     | Dano inicial do projétil      |
| `upgrade_cost_mult`   | float  | 1.15    | Multiplicador global de custo |
| `hp_scale_factor`     | float  | 1.08    | Escala de HP por stage        |
| `offline_efficiency`  | float  | 0.25    | % do ganho online             |
| `offline_cap_hours`   | int    | 8       | Máximo de horas offline       |
| `spawn_rate_min`      | float  | 0.1     | Intervalo mínimo de spawn     |

---

## Notas de Balanceamento

### Armadilhas Comuns

1. **Exponencial demais:** Se HP escala mais rápido que dano, jogador fica "stuck"
2. **Linear demais:** Jogo fica trivial, sem sensação de progressão
3. **Upgrade trap:** Um upgrade claramente superior torna outros inúteis
4. **Wall artificial:** Ponto onde progresso para completamente

### Checklist de Sanidade

- [ ] Primeiro upgrade comprável em < 10 segundos?
- [ ] Sempre existe um upgrade "alcançável" em < 2 minutos?
- [ ] Nenhum upgrade é > 10x mais eficiente que outros?
- [ ] Ganhos offline são satisfatórios mas não quebram o jogo?
- [ ] Números fazem sentido visualmente (não muito spam de dígitos)?