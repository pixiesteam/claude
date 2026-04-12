---
name: pm-competitive-orchestrator
description: >
  Orchestrates competitive intelligence — mapping the market, profiling competitors, running SWOT analysis, and translating findings into positioning. Use whenever a PM mentions competitive analysis, a competitor, market intelligence, win/loss analysis, how to compete, or positioning against alternatives. Also triggers for: "analisar concorrente", "inteligência competitiva", "como competir com X", "concorrente lançou X", "win/loss", "posicionamento competitivo". This orchestrator calls independent PM skills in sequence — each skill must be installed separately.
---

# PM Competitive Orchestrator

Guides the PM through competitive intelligence: from mapping who's out there to profiling key players to translating findings into actionable positioning.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | When |
|------------|------|
| `pm-competitive-profile` | Deep-dive on a specific competitor |
| `pm-swot-analysis` | Strategic analysis with TOWS actions |
| `pm-positioning-statement` | Translate competitive intel into positioning |

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
context/competitors.md  → existing intel, win/loss themes
context/product.md      → current positioning
context/company.md      → strategic priorities
context/personas.md     → target customers for positioning
```

State what you found, then identify the entry point:

**Entry A — Specific competitor triggered this**
> "A competitor just launched X" or "We keep losing to Y". Start with `pm-competitive-profile` for that competitor.

**Entry B — Strategic planning input needed**
> "Annual planning" or "Where should we focus?" Start with `pm-swot-analysis`.

**Entry C — Positioning needs updating**
> "Our messaging isn't working" or "Sales needs a better story". Start with `pm-positioning-statement`.

**Entry D — Full cycle**
> Start cold, no existing intel. Run A → B → C in sequence.

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
✓ O conteúdo reflete o que você está vendo no mercado?
✓ Há concorrentes, movimentos ou dados que ficaram de fora?
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
| `pm-competitive-profile` | `pm-swot-analysis` | Competitor strengths, moats, weaknesses |
| `pm-swot-analysis` | `pm-positioning-statement` | TOWS strategic actions, competitive threats |
| Any | `pm-positioning-statement` | Competitive alternatives (required input) |

State what you're carrying forward:
> "I'm using the DHM analysis from the Monday.com profile as the 'threats' input for the SWOT. You won't need to re-describe their moats."

---

## Full competitive intelligence cycle

```
pm-competitive-profile (for each key competitor)
  → [GATE] →
pm-swot-analysis
  → [GATE] →
pm-positioning-statement
```

Multiple competitor profiles can be run before moving to SWOT. Each profile goes through its own gate before moving to the next.

---

## Output files (PT-BR)

```
competitive/outputs/
  perfil-[competitor]-[YYYY-MM-DD].md
  swot-[topic]-[YYYY-MM-DD].md
  posicionamento-[YYYY-MM-DD].md
```

---

## Bridge to strategy

After positioning is approved:
> "You now have competitive intel and a positioning statement. These are the primary inputs for `pm-strategy-orchestrator` — specifically for the North Star and Go-to-Market skills."
