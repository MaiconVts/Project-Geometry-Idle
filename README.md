# Project Geometry Idle

> ⚠️ **Status:** Arquitetura & Prototipagem — Nenhum design visual definido ainda.

Simulação de física idle/clicker com foco em satisfação visual e progressão exponencial.

---

## Sobre o Projeto

Este repositório contém a **estrutura base** para um idle game de física. Toda a arquitetura está em desenvolvimento ativo e sujeita a mudanças.

### O que ESTÁ definido

- Arquitetura de código (padrões, estrutura de pastas)
- Sistemas core (save, pooling, eventos)
- Estratégias de persistência e segurança
- Tech stack (Unity 6, C#, Box2D)

### O que NÃO está definido

| Elemento       | Exemplos em aberto                                    |
|----------------|-------------------------------------------------------|
| Moeda          | Ouro? Cristais? Polígonos? Energia?                   |
| Projéteis      | Setas? Bombas? Shurikens? Lasers?                     |
| Alvos          | Formas geométricas? Criaturas? Estruturas?            |
| Tema visual    | Minimalista? Neon? Medieval? Sci-fi?                  |

O código usa **nomes genéricos** (`Projectile`, `Target`, `Currency`) para permitir qualquer direção de design.

---

## Quick Start

### Requisitos

- Unity 6 LTS (ou 2022.3 LTS)
- Visual Studio 2022
- Git + Git LFS

### Setup

```bash
git clone https://github.com/seu-usuario/geometry-idle.git
cd geometry-idle
git lfs install
git lfs pull
```

Abra o projeto no Unity Hub e aguarde a importação.

---

## Documentação

Toda a documentação técnica e de design está em `/docs`:

```
docs/
├── 01-Design/
│   ├── 01-Game_Pitch.md          # Visão executiva do projeto
│   ├── 02-GDD_Game_Design_Document.md  # Regras e mecânicas
│   └── 03-Economy_And_Math.md    # Fórmulas e balanceamento
├── 02-Engineering/
│   ├── 01-TDD_Architecture.md    # Arquitetura técnica
│   ├── 02-Data_Persistence_Security.md  # Save e anti-cheat
│   └── 03-Code_Style_Guide.md    # Padrões de código
├── 03-Art_UX/
│   ├── 01-UI_UX_Flow.md          # Fluxo de navegação
│   └── 02-Art_Bible.md           # Direção artística
└── 04-Production/
    ├── 01-QA_Performance_Plan.md # Metas de qualidade
    ├── 02-Roadmap_do_Projeto.md  # Cronograma
    └── 03-DevLog_e_PostMortem.md # Registro de desenvolvimento
```

---

## Estimativa

| Fase                        | Duração   |
|-----------------------------|-----------|
| Core Loop & Física          | 2 semanas |
| Arquitetura (Save/Events)   | 2 semanas |
| Game Design & UI            | 3 semanas |
| Polimento & Ferramentas     | 2 semanas |
| **Total**                   | 8-10 semanas |

---

## Tech Stack

| Categoria   | Tecnologia                 |
|-------------|----------------------------|
| Engine      | Unity 6 LTS                |
| Linguagem   | C# (.NET Standard 2.1)     |
| Física      | Box2D (Unity 2D nativo)    |
| IDE         | Visual Studio 2022         |
| Backend     | Unity Gaming Services      |

---

## Contribuindo

1. Crie uma branch: `git checkout -b feat/minha-feature`
2. Siga o [Code Style Guide](docs/02-Engineering/03-Code_Style_Guide.md)
3. Commits semânticos: `feat(scope) - descrição`
4. Abra um Pull Request

---

## Licença

A definir.