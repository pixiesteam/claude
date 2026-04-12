---
name: pm-strategy-orchestrator
description: >
  Orchestrates product strategy definition — North Star metric, value proposition, metrics framework,
  pricing, and go-to-market. Use whenever a PM needs to define or redefine product direction, set
  the measurement system, evaluate pricing, or build a GTM plan. Also triggers for: "definir
  estratégia", "direção do produto", "north star", "framework de métricas", "estratégia de pricing",
  "go-to-market", "o que medir", "como crescer". This orchestrator calls independent PM skills in
  sequence — each skill must be installed separately.
---

# PM Strategy Orchestrator

Guides the PM through product strategy definition: what the product is for, who it's for, how to
measure success, and how to monetize and launch it.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | When |
|------------|------|
| `pm-north-star` | Define the one metric that captures core value |
| `pm-value-prop-canvas` | Map user needs vs. product fit |
| `pm-metric-framework` | Build the full measurement pyramid |
| `pm-pricing-strategy` | Evaluate and redesign pricing |
| `pm-go-to-market` | Plan a launch or market expansion |
| `pm-positioning-statement` | Define positioning (shared with competitive) |

---

## Step 0 — Check context files (ALWAYS FIRST)

Before routing to any skill, read the available context files:

```
context/product.md       → value prop, roadmap, metrics, known issues
context/company.md       → strategic priorities, OKRs, team structure
context/personas.md      → user segments, jobs-to-be-done, pains
context/competitors.md   → competitive intelligence
goals.md                 → current OKRs and quarterly targets
context/backlog.md       → prioritized initiatives and current sprint
```

**If no context files exist:**
> "Não encontrei arquivos de contexto do projeto. Quer rodar o `pm-context-builder` primeiro?
> Isso leva 15–20 min e faz com que todas as skills sejam mais precisas e menos repetitivas."

- "Sim" → run `pm-context-builder`, then return here
- "Não, pode continuar" → proceed with what's available, ask only for genuinely missing info

**If context files exist:**
Load silently. State what you found in one line before asking anything:
> "Contexto carregado: [produto] — [fase/OKR principal]. O que você precisa?"

**Update protocol:** When a skill output reveals new information relevant to context files,
present the context update prompt at the gate. Apply only what the user approves.

---

## How to start

Read context files first:
```
context/product.md       → value prop, current metrics, features
context/personas.md      → user segments, jobs-to-be-done
context/company.md       → business model, OKRs, priorities
context/competitors.md   → competitive alternatives (for positioning)
strategy/outputs/        → any previous strategy outputs
```

State what you found, then ask: **"What's driving this conversation? New product, pivot, or specific gap in the current strategy?"**

**Entry A — Starting fresh (new product or major pivot)**
> Full cycle: North Star → Value Prop Canvas → Metric Framework → Pricing → GTM

**Entry B — Measurement gap**
> "We're tracking too many things and aligning on none." Start with `pm-north-star` → `pm-metric-framework`.

**Entry C — Pricing not working**
> Start directly with `pm-pricing-strategy`.

**Entry D — Launch imminent**
> Start directly with `pm-go-to-market`. (If positioning is unclear, run `pm-positioning-statement` first.)

**Entry E — Fit diagnosis**
> Start with `pm-value-prop-canvas` to audit whether the product is solving the right problems.

---

## MANDATORY GATE PROTOCOL

Every skill runs inside this loop — no exceptions:

```
1. Call the skill → deliver its output
2. STOP completely
3. Present the gate prompt below
4. Wait for explicit approval
5. Only then call the next skill
```

**NEVER chain two skills in the same response.**
**NEVER interpret enthusiasm as approval.**
**NEVER suggest the next skill inside a skill output.**

### Gate prompt (use after every skill output)

```
---
REVISÃO NECESSÁRIA — [Nome da Skill Concluída]

Antes de avançar:
✓ O conteúdo reflete a direção estratégica que você tem em mente?
✓ Há algo que precisa ser ajustado, adicionado ou removido?
✓ Os itens marcados com ⚠️ estão corretos?

Responda com:
— "Aprovado" para avançar para [próxima skill]
— "Ajustar: [o que mudar]" para corrigir antes de avançar
— "Parar aqui" para encerrar o ciclo neste ponto
---
```
### Context update prompt (add to gate when skill reveals new project information)

```
---
ATUALIZAÇÃO DE CONTEXTO — [Nome da Skill Concluída]

Esta skill revelou informação nova que pode enriquecer os arquivos de contexto:
→ [Informação descoberta — ex: "novo segmento de usuário identificado"]
→ Arquivo sugerido: [context/personas.md / context/product.md / goals.md / etc.]

Quer atualizar agora?
— "Sim" → atualizo o arquivo e registro a mudança
— "Não" → registro na skill output mas não altero o contexto
---
```



---

## Iteration limit

Each skill has a maximum of **3 revision rounds**.
After round 3 on the same skill:
> "We've done 3 rounds of revision on this document. Do you want to keep refining, reset this skill's scope from scratch, or move forward with the current version?"

---

## Context handoff between skills

| From | To | What passes forward |
|------|----|---------------------|
| `pm-north-star` | `pm-metric-framework` | North Star metric and definition |
| `pm-value-prop-canvas` | `pm-north-star` | Core value to capture in metric |
| `pm-metric-framework` | `pm-go-to-market` | Success metrics for GTM measurement |
| `pm-positioning-statement` | `pm-go-to-market` | Messaging by persona, competitive frame |

State what you're carrying forward:
> "I'm using the North Star from the previous step as the anchor for the metrics framework. You won't need to redefine it."

---

## Full strategy cycle sequence

```
pm-value-prop-canvas → [GATE] → pm-north-star → [GATE] → pm-metric-framework
                                                            → [GATE] →
                                                         pm-pricing-strategy
                                                            → [GATE] →
                                                         pm-go-to-market
```

For positioning: run `pm-positioning-statement` (from competitive orchestrator or standalone) before `pm-go-to-market`.

---

## Output files (PT-BR)

```
strategy/outputs/
  north-star-[YYYY-MM-DD].md
  canvas-proposta-valor-[YYYY-MM-DD].md
  framework-metricas-[YYYY-MM-DD].md
  analise-pricing-[YYYY-MM-DD].md
  gtm-[produto]-[YYYY-MM-DD].md
  posicionamento-[YYYY-MM-DD].md
```

---

## Bridge to planning

After strategy is defined:
> "You now have the strategic direction defined. This feeds directly into `pm-planning-orchestrator` — the business objectives from the GTM and pricing work become the inputs for the strategic review and roadmap."
