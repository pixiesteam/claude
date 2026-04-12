---
name: pm-context-builder
description: >
  PM skill: Create and maintain the context files that all PM skills read automatically — product.md, company.md, personas.md, competitors.md, goals.md, and backlog.md. Run this first before using any PM orchestrator or skill. The richer these files, the less you need to explain in every session. Triggers for: "setup context", "create context files", "pm context", "configurar contexto", "criar arquivos de contexto", "setup inicial", "começar a usar as skills", "pm os setup", "what context files do I need".
---

# PM Context Builder

Creates and maintains the six context files that all PM skills read automatically. Run this once to set up, then update incrementally as your product and company evolve.

**Language:** Conversation in English. Output files in Portuguese (Brazil) where content is PT-BR, English where content is EN.
**Output location:** `context/` folder

---

## Why this matters

Every PM skill in this system reads context files before asking you any questions. The more complete these files are, the fewer questions you'll be asked — and the more grounded every output will be in your actual product reality.

Without context files: skills ask you everything, every time.
With context files: skills pull what they need silently and ask only for what's genuinely missing.

---

## The six context files

| File | What it contains | Updated when |
|------|-----------------|--------------|
| `context/product.md` | Value prop, roadmap, metrics, known issues, pricing, activation rate | After planning cycles, when metrics change |
| `context/company.md` | Strategic priorities, OKRs, team structure, business model, capacity | Quarterly or when strategy shifts |
| `context/personas.md` | User segments, jobs-to-be-done, pains, gains, aha moments | After discovery research |
| `context/competitors.md` | Competitor profiles, win/loss themes, pricing, feature gaps | When competitive landscape shifts |
| `goals.md` | Current OKRs, quarterly targets, success metrics, blockers | Every quarter |
| `context/backlog.md` | Prioritized initiatives and tasks, current sprint items | Weekly or after planning |

---

## Process

Run this skill in two modes:

**Mode A — First setup (no context files exist)**
Interview the PM across all six files, then generate each one.

**Mode B — Update (files exist, need refreshing)**
Read existing files, show what's outdated or missing, ask only for the gaps.

---

### Mode A: First Setup

Tell the PM what you're doing:
> "I'm going to ask you a series of questions across six areas. This will take about 15–20 minutes. The output is six files that all PM skills will read automatically — you'll never need to explain your product, company, or users from scratch again."

Run the interviews in this order. Ask each section as a single conversational message — not one question at a time.

---

#### Interview 1: Product (`context/product.md`)

Ask:

> "Let's start with your product. Tell me:
>
> 1. **What does your product do?** One sentence — what problem does it solve and for whom?
> 2. **What's your current roadmap?** What are you building Now, Next, and Later?
> 3. **What are your key metrics?** (activation rate, retention, NPS, ARR, churn — whatever you track)
> 4. **What are the known issues or pain points in the product?** What do users complain about most?
> 5. **What's your pricing model?** (tiers, price points, model type)
> 6. **What's your current value proposition?** How do you describe what makes you different?"

---

#### Interview 2: Company (`context/company.md`)

Ask:

> "Now your company context:
>
> 1. **What are your top 2–3 strategic priorities right now?** What does the business most need to achieve?
> 2. **What's your business model?** (subscription, usage-based, transaction, etc.)
> 3. **What are your team's OKRs or goals this quarter?** (if you have them)
> 4. **What's your team structure?** How many people in product, engineering, design?
> 5. **What stage is the company?** (seed, series A/B/C, growth, enterprise)
> 6. **What's your sales motion?** (product-led, sales-led, or hybrid)"

---

#### Interview 3: Personas (`context/personas.md`)

Ask:

> "Now your users:
>
> 1. **Who are your main user segments?** Describe 1–3 types of people who use your product.
> 2. **For each persona: what's their primary job-to-be-done?** What are they trying to accomplish?
> 3. **What are their biggest pains and frustrations?** What makes their job hard?
> 4. **What's the 'aha moment' for each persona?** When do they first feel the product's value?
> 5. **Any notable quotes from users?** Real language you've heard in interviews or support."

---

#### Interview 4: Competitors (`context/competitors.md`)

Ask:

> "Now your competitive landscape:
>
> 1. **Who are your top 2–4 direct competitors?** The ones that come up most in deals or comparisons.
> 2. **For each: what do they do well?** Why do customers choose them?
> 3. **What are your main win themes?** Why do you win deals against them?
> 4. **What are your main loss themes?** Why do you lose to them?
> 5. **What's the rough pricing of your main competitors?** (doesn't need to be exact)
> 6. **Any recent moves from competitors worth noting?** New features, funding, pivots?"

---

#### Interview 5: Goals (`goals.md`)

Ask:

> "Now your current goals:
>
> 1. **What are your OKRs or goals this quarter?** (Objectives + Key Results if you use OKRs, or just top priorities)
> 2. **What metrics are you tracking against those goals?** What's the current baseline and target for each?
> 3. **What are the biggest blockers to hitting your goals?** What's getting in the way?
> 4. **What's the planning period?** (e.g., Q2 2026)"

---

#### Interview 6: Backlog (`context/backlog.md`)

Ask:

> "Finally, your backlog:
>
> 1. **What are the top 5–10 initiatives or features you're considering building?** (doesn't need to be formal — a list is fine)
> 2. **What's in your current sprint or being worked on right now?**
> 3. **What's explicitly on hold or deferred?** Things you've said 'not now' to."

---

### After all interviews: generate the files

Generate all six files sequentially. Use the templates below. After each file, say:
> "Here's `[filename]`. Review it — does this capture your situation accurately? Anything to adjust before I move to the next file?"

Wait for confirmation before generating the next file.

---

## Output templates

### `context/product.md`

```markdown
# Product Context

**Última atualização:** [Data]

## O Que o Produto Faz

[Uma frase: qual problema resolve e para quem]

## Proposta de Valor

[Como você se diferencia — em linguagem de cliente]

## Roadmap Atual

### Now (fazendo agora)
- [Iniciativa 1]
- [Iniciativa 2]

### Next (fazendo em seguida)
- [Iniciativa 1]

### Later (planejado para depois)
- [Iniciativa 1]

## Métricas-Chave

| Métrica | Valor Atual | Meta | Última Atualização |
|---------|------------|------|-------------------|
| [Métrica] | [Valor] | [Meta] | [Data] |

## Problemas Conhecidos

- [Problema 1 — o que usuários reclamam]
- [Problema 2]

## Pricing

| Tier | Preço | Público |
|------|-------|---------|
| [Tier] | [Preço] | [Quem] |

**Modelo:** [Seat-based / Usage-based / Flat fee / Hybrid]

## Taxa de Ativação

**Atual:** [X]%
**Meta:** [Y]%
**Momento aha definido:** [A ação específica que prova valor]
```

---

### `context/company.md`

```markdown
# Company Context

**Última atualização:** [Data]

## Prioridades Estratégicas

1. [Prioridade 1]
2. [Prioridade 2]
3. [Prioridade 3]

## Modelo de Negócio

[Subscription / Usage-based / Transaction — com detalhes relevantes]

## Estágio

[Seed / Series A / Series B / Growth / Enterprise]

## Movimento de Vendas

[Product-led / Sales-led / Hybrid — com detalhes]

## Estrutura do Time

| Função | Headcount |
|--------|-----------|
| Produto | [N] |
| Engenharia | [N] |
| Design | [N] |
| Vendas | [N] |
| CS | [N] |

## OKRs do Quarter

**Quarter:** [Q e Ano]

### Objetivo 1: [Nome]
- KR 1: [Baseline] → [Meta]
- KR 2: [Baseline] → [Meta]

### Objetivo 2: [Nome]
- KR 1: [Baseline] → [Meta]

## Restrições Conhecidas

- [Restrição 1 — ex: sem nova infraestrutura antes do Series B]
- [Restrição 2]
```

---

### `context/personas.md`

```markdown
# Personas

**Última atualização:** [Data]

---

## Persona 1: [Nome] — [Papel/Título]

**Empresa típica:** [Tamanho, tipo, setor]
**Responsabilidades:** [O que essa pessoa faz]

### Job-to-be-Done Principal

> "When [situação], I want to [ação], so I can [resultado]."

### Jobs Secundários

- [Job 2]
- [Job 3]

### Maiores Dores

1. [Dor 1 — com linguagem do usuário se possível]
2. [Dor 2]
3. [Dor 3]

### Momento Aha

[A ação específica quando essa persona sente o valor do produto pela primeira vez]

### Citações Reais

> "[Citação de entrevista ou suporte]"
> "[Citação 2]"

### Processo de Compra

[Como essa persona descobre, avalia e compra — quem mais está envolvido]

---

## Persona 2: [Nome] — [Papel/Título]

[Mesma estrutura]
```

---

### `context/competitors.md`

```markdown
# Competitive Intelligence

**Última atualização:** [Data]

---

## [Concorrente 1]

**Website:** [URL]
**Em uma frase:** [O que fazem]
**Estágio:** [Startup / Growth / Enterprise]
**Funding:** [Rodada / Valor se conhecido]
**Público-alvo:** [Quem atendem]

**Pricing:** [Tiers e valores aproximados]

**O que fazem bem (por que clientes escolhem eles):**
- [Ponto 1]
- [Ponto 2]

**Fraquezas (onde falham):**
- [Fraqueza 1]
- [Fraqueza 2]

**Nossos temas de vitória contra eles:**
- [Quando ganhamos]

**Nossos temas de derrota para eles:**
- [Quando perdemos]

**Movimentos recentes:**
- [Data]: [O que fizeram — lançamento, funding, mudança de pricing]

---

## [Concorrente 2]

[Mesma estrutura]

---

## Matriz de Features

| Feature | Nós | [Comp 1] | [Comp 2] |
|---------|-----|---------|---------|
| [Feature] | ✅ | ✅ | ❌ |
| [Feature] | ✅ | ❌ | ✅ |
```

---

### `goals.md`

```markdown
# Goals & OKRs

**Quarter:** [Q e Ano]
**Última atualização:** [Data]

---

## OKRs do Quarter

### Objetivo 1: [Nome inspiracional]

| Key Result | Baseline | Meta | Atual | Status |
|-----------|----------|------|-------|--------|
| [KR 1] | [X] | [Y] | [Atual] | 🟢/🟡/🔴 |
| [KR 2] | [X] | [Y] | [Atual] | 🟢/🟡/🔴 |

### Objetivo 2: [Nome]

| Key Result | Baseline | Meta | Atual | Status |
|-----------|----------|------|-------|--------|
| [KR 1] | [X] | [Y] | [Atual] | 🟢/🟡/🔴 |

---

## Maiores Bloqueadores

1. [Bloqueador 1 — o que está impedindo o progresso]
2. [Bloqueador 2]

---

## Histórico de OKRs Anteriores

### [Q anterior]
- [Objetivo 1]: [Resultado — 0.7 / Não atingiu / Atingiu]
- [Objetivo 2]: [Resultado]
```

---

### `context/backlog.md`

```markdown
# Backlog

**Última atualização:** [Data]

---

## Em Andamento (Sprint Atual)

| Item | Owner | Status |
|------|-------|--------|
| [Item] | [Nome] | Em progresso |

---

## Próximas Iniciativas (P0/P1)

| Item | Prioridade | Tamanho est. | Notas |
|------|-----------|-------------|-------|
| [Item] | P0 | M | [Contexto] |
| [Item] | P1 | G | [Contexto] |

---

## Backlog (P2/P3)

| Item | Prioridade | Por que ainda não |
|------|-----------|------------------|
| [Item] | P2 | [Motivo] |

---

## Explicitamente Adiado

| Item | Motivo | Quando revisar |
|------|--------|----------------|
| [Item] | [Motivo claro] | [Q ou trigger] |
```

---

## Mode B: Updating existing files

If context files already exist:

1. Read all six files silently
2. Show a status summary:

```
CONTEXTO ATUAL — Status

context/product.md    ✅ Atualizado (há 2 semanas)
context/company.md    ⚠️  Pode estar desatualizado (há 3 meses)
context/personas.md   ✅ Atualizado (há 1 mês)
context/competitors.md ❌ Vazio ou muito incompleto
goals.md              ⚠️  Quarter anterior ainda no arquivo
context/backlog.md    ✅ Atualizado
```

3. Ask only about outdated or missing sections
4. Update the relevant files

---

## Maintenance reminders

Each skill will suggest updating context files when it finds relevant new information. Common triggers:

- After a planning cycle → update `context/product.md` (roadmap) and `goals.md`
- After discovery research → update `context/personas.md`
- After a competitive win/loss → update `context/competitors.md`
- After a strategy shift → update `context/company.md`
- After sprint planning → update `context/backlog.md`

---

## Cross-system usage

Context files created here are also consumed by agents in other pipelines:

| File | Also read by | What they extract |
|------|-------------|-------------------|
| `context/personas.md` | `pd-ux-artifacts-agent` | Personas and JTBD as input for Empathy Maps and Journey Maps |
| `context/personas.md` | `pd-research-agent` | Known jobs to build focused research guides, not rediscover what's documented |
| `context/personas.md` | `pd-strategy-agent` | Real user pains and jobs to anchor problem statements and opportunity matrices |
| `context/personas.md` | `component-orchestrator` | "For whom is this component" — skips that interview question |
| `context/product.md` | `pd-strategy-agent` | Current metrics as baselines for design success criteria |

**This means:** the richer your `context/personas.md`, the better every design artifact produced
by the Product Designer pipeline will be — without the designer having to re-enter any of it.
The PM's discovery work flows directly into the designer's output quality.

---

## Proactive update protocol

Orchestrators will prompt you at each gate if new findings should update context files. When this happens:

**Accept the update:** Say "Yes, update [filename]" and the orchestrator will apply the changes and confirm what was added or changed.

**Decline:** Say "No, not now" — the finding is noted in the skill output but context files are not changed.

**Suggested update format in gates:**
```
Os achados desta skill revelaram algo novo:
— [Novo job identificado] → sugiro adicionar a context/personas.md
— [Novo dado de concorrente] → sugiro adicionar a context/competitors.md

Quer que eu atualize agora?
```

This keeps context files current without requiring you to remember to update them manually after every session.

---

## Context file health indicators

When reading context files, skills will flag staleness:

- **🟢 Current** — updated within 4 weeks
- **🟡 May be stale** — last updated 1–3 months ago
- **🔴 Likely outdated** — no update in 3+ months or quarter has changed

A 🔴 flag doesn't block the skill — it warns you that the output will be less personalized.

---

*Entrega concluída. O orquestrador apresentará o gate de revisão.*
