---
name: pm-planning-orchestrator
description: >
  Orchestrates the full PM planning cycle — from strategic retrospective to tactical roadmap to quarterly plan to prioritized backlog. Use this skill whenever a PM mentions planning, roadmap, quarterly cycle, priorities, what to build next, OKRs, capacity, or anything related to deciding direction. Also triggers for: "planejar o quarter", "montar o roadmap", "priorizar o backlog", "planejamento estratégico", "o que construir", "o que focar", "revisão de quarter". This orchestrator calls independent PM skills in sequence — each skill must be installed separately.
---

# PM Planning Orchestrator

Facilitates the full planning cycle, calling the right PM skills in the right sequence and passing context between them.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | Step |
|------------|------|
| `pm-strategic-review` | 1 — look back before looking forward |
| `pm-roadmap-builder` | 2 — translate direction into initiatives |
| `pm-quarterly-plan` | 3 — break initiatives into quarterly bets |
| `pm-backlog-prioritizer` | 4 — score and sequence the work |
| `pm-okr-coach` | conditional — write OKRs when goals are undefined |

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

Before anything, read these context files if they exist:
```
context/product.md     → current roadmap, metrics, known issues
context/company.md     → strategic priorities, team structure
context/personas.md    → user segments and jobs-to-be-done
context/competitors.md → competitive landscape
strategy/outputs/      → previous planning outputs
```

State what you found, then identify the entry point:

**Full cycle** — no prior planning artifacts exist → start with `pm-strategic-review`
**Mid-cycle** — roadmap exists, no quarterly plan → start with `pm-quarterly-plan`
**Backlog only** — initiatives are clear, need sequencing → start with `pm-backlog-prioritizer`
**Single skill** — PM asks for one specific output → run that skill directly

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

### Standard gate prompt (use after most skill outputs)

```
---
REVISÃO NECESSÁRIA — [Nome da Skill Concluída]

Antes de avançar:
✓ O conteúdo reflete o que você tem em mente?
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

## CONDITIONAL ROUTING — after pm-strategic-review

After delivering the strategic review output and the PM approves it, **do not automatically proceed to pm-roadmap-builder**. Instead, read the content of the approved output and evaluate which of these three signals is most present:

### Signal 1: Strategy is undefined
**Present when the strategic review reveals that objectives are vague, the team has conflicting priorities, or there's no clear "what does success look like" for the product.**

Look for: business objectives section with ⚠️ flags, language like "unclear," "to be defined," "no consensus," multiple conflicting strategic directions, absence of measurable product outcomes, or a "not doing" list that's empty (nothing being said no to is a sign nothing is prioritized).

**Route: suggest pm-strategy-orchestrator before roadmap-builder**

Present this at the gate:

```
---
ROTEAMENTO — pm-strategic-review concluído

A revisão estratégica revelou que a direção do produto ainda não está clara o suficiente
para construir um roadmap defensável. Construir o roadmap agora produziria uma lista de
features sem âncora estratégica — o tipo de roadmap que muda toda semana.

Caminho recomendado antes do roadmap:
→ pm-strategy-orchestrator — definir north star, proposta de valor e direção de produto

Após isso, volte aqui para construir o roadmap com base estratégica sólida.

O que você prefere?
— "Sim, vamos definir a estratégia primeiro" → pausar planejamento, rodar pm-strategy-orchestrator
— "Não, a estratégia está clara o suficiente — avançar com o roadmap" → continuar para pm-roadmap-builder
— "Parar aqui" para encerrar
---
```

**If the PM chooses to proceed with the roadmap anyway:** acknowledge and proceed, but add this note at the top of the roadmap output:
> "⚠️ Este roadmap foi construído com direção estratégica parcialmente definida. Revisitar após rodar pm-strategy-orchestrator para validar que as iniciativas ainda fazem sentido."

---

### Signal 2: OKRs are missing or too vague to anchor the roadmap
**Present when the strategic review shows that the team has direction but no measurable goals — roadmap initiatives can't be connected to outcomes.**

Look for: product outcomes section with no metrics or with placeholder targets, OKRs section that's empty or marked ⚠️, goals described as activities ("launch X," "build Y") rather than outcomes ("increase activation to Z%"), or company.md and goals.md showing no current quarter OKRs.

**Route: pm-okr-coach before pm-roadmap-builder**

Present this at the gate:

```
---
ROTEAMENTO — pm-strategic-review concluído

A revisão estratégica mostra que a direção existe, mas não há OKRs mensuráveis
para ancorar as iniciativas do roadmap. Sem isso, o roadmap não tem como ser
priorizado por impacto — só por preferência.

Caminho recomendado:
→ pm-okr-coach — escrever OKRs com baseline → meta para o quarter
→ pm-roadmap-builder — construir roadmap conectado a esses outcomes

O que você prefere?
— "Sim, definir OKRs primeiro" → rodar pm-okr-coach → depois pm-roadmap-builder
— "Pular OKRs, avançar com roadmap" → continuar para pm-roadmap-builder diretamente
— "Parar aqui" para encerrar
---
```

---

### Signal 3: Strategy is clear — proceed normally

**Route: pm-roadmap-builder (standard path)**

Present this at the gate:

```
---
ROTEAMENTO — pm-strategic-review concluído

A direção estratégica está clara. Os próximos passos seguem o fluxo padrão.

Próxima skill: pm-roadmap-builder
O que passa: objetivos de negócio + outcomes de produto + lista "não faremos"

Aprovado para avançar?
— "Aprovado" para chamar pm-roadmap-builder
— "Ajustar: [o que mudar]" para corrigir antes de avançar
— "Parar aqui" para encerrar
---
```

---

## Context handoff between skills

State what you're carrying forward explicitly:
> "I'm carrying forward the business objectives and product outcomes from the strategic review. You won't need to re-answer those questions."

| From | To | What passes forward |
|------|----|---------------------|
| `pm-strategic-review` | `pm-okr-coach` | Business objectives, gaps in measurable goals |
| `pm-strategic-review` | `pm-roadmap-builder` | Business objectives, product outcomes, not-doing list |
| `pm-okr-coach` | `pm-roadmap-builder` | OKRs with baselines and targets as outcome anchors |
| `pm-roadmap-builder` | `pm-quarterly-plan` | Now/Next/Later initiatives with owners |
| `pm-quarterly-plan` | `pm-backlog-prioritizer` | Strategic themes and bets |

---

## Full planning cycle sequences

**Standard path (strategy clear):**
```
pm-strategic-review → [CONDITIONAL GATE: clear] → pm-roadmap-builder → [GATE] → pm-quarterly-plan → [GATE] → pm-backlog-prioritizer
```

**OKRs-first path:**
```
pm-strategic-review → [CONDITIONAL GATE: OKRs missing] → pm-okr-coach → [GATE] → pm-roadmap-builder → [GATE] → pm-quarterly-plan → [GATE] → pm-backlog-prioritizer
```

**Strategy-first path (redirect):**
```
pm-strategic-review → [CONDITIONAL GATE: strategy unclear] → [pause → pm-strategy-orchestrator] → return → pm-roadmap-builder
```

---

## Output files (PT-BR)

```
strategy/outputs/
  revisao-estrategica-[YYYY-MM-DD].md
  roadmap-[YYYY-MM-DD].md
  plano-trimestral-[YYYY-MM-DD].md
  priorizacao-backlog-[YYYY-MM-DD].md
  okrs-[YYYY-MM-DD].md
```
