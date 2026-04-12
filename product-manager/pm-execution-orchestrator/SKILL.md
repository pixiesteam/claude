---
name: pm-execution-orchestrator
description: >
  Orchestrates the execution cycle — from problem to spec to sprint-ready work. Use whenever a PM needs to write a PRD, create a tech spec, break work into user stories, plan a sprint, or assess risks. Also triggers for: "escrever prd", "especificação técnica", "user stories", "planejamento de sprint", "registro de riscos", "como construir X", "transformar pesquisa em spec", "preparar para engenharia". This orchestrator calls independent PM skills in sequence — each skill must be installed separately.
---

# PM Execution Orchestrator

Guides the PM through the execution cycle: from a defined opportunity to a spec the engineering team can build from.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | Step | When |
|------------|------|------|
| `pm-prd` | 1 | Translate a problem into a buildable spec |
| `pm-tech-spec` | 2 | Define architecture, data model, and APIs |
| `pm-user-stories` | 3 | Break the PRD into sprint-ready backlog items |
| `pm-sprint-planning` | 4 | Plan the sprint with capacity and committed work |
| `pm-risk-register` | conditional | Inserted before tech-spec when PRD has high uncertainty |
| `pm-decision-doc` | any | Document significant decisions (shared with communication) |
| `pm-one-pager` | any | Quick alignment brief (shared with communication) |

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
context/product.md       → roadmap, current features, known issues
context/personas.md      → who the feature is for
context/company.md       → strategic priorities, team structure
discovery/outputs/       → JTBD, OST, or research that feeds this PRD
```

State what you found, then identify the entry point:

**Entry A — Starting from a problem or opportunity**
> Research exists, need to write the PRD. Start with `pm-prd`.

**Entry B — PRD exists, need to spec it technically**
> PRD is approved. Start with `pm-tech-spec`.

**Entry C — PRD and tech spec exist, need to break into stories**
> Ready for grooming. Start with `pm-user-stories`.

**Entry D — Stories exist, need to plan the sprint**
> Start with `pm-sprint-planning`.

**Entry E — Risk assessment needed standalone**
> Start with `pm-risk-register` directly.

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
✓ O conteúdo reflete o que você quer construir?
✓ Há requisitos, restrições ou edge cases que ficaram de fora?
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

## CONDITIONAL ROUTING — after pm-prd

After delivering the PRD output and the PM approves it, **do not automatically proceed to pm-tech-spec**. Instead, count the ⚠️ flags in the Evidence and Open Questions sections of the approved PRD and evaluate which of these signals is most present:

### Signal 1: Evidence is weak — redirect to discovery
**Present when the PRD has 3 or more ⚠️ Assumido flags in the Evidence section, no real customer quotes, or the problem statement is not grounded in validated research.**

Look for: multiple "⚠️ Assumido — validar com [dado]" entries in the Evidence table, Open Questions section with unknowns that are foundational (not edge cases), confidence below 50% on success criteria, or no data from `discovery/outputs/` referenced anywhere in the PRD.

**Why this matters:** Specing an unvalidated problem is the most expensive mistake in product development. Discovery now — even 1–2 weeks — is always cheaper than building the wrong thing. A tech spec on top of a weak PRD multiplies the waste.

**Route: redirect to pm-discovery-orchestrator**

Present this at the gate:

```
---
ROTEAMENTO — pm-prd concluído

O PRD contém [N] suposições sem evidência real (⚠️ Assumido).
Avançar para a spec técnica agora significa construir arquitetura
sobre um problema ainda não validado.

Recomendação: pausar execution e rodar discovery primeiro.

O que você quer fazer?
— "Ir para discovery" → pausar aqui, rodar pm-discovery-orchestrator com foco em validar
  as suposições do PRD. Volte com evidência real e atualize o PRD antes de especificar.
— "Avançar mesmo assim" → continuar para pm-tech-spec, mas o PRD será marcado com:
  ⚠️ Spec construída sobre problema não validado — revisitar após discovery.
— "Parar aqui" para encerrar
---
```

**If the PM chooses to proceed anyway:** add this warning block at the top of the tech spec output:
> "⚠️ Esta spec foi construída sobre um PRD com múltiplas suposições não validadas. Risco de retrabalho alto. Considere rodar `pm-discovery-orchestrator` antes de começar o desenvolvimento."

---

### Signal 2: High uncertainty — insert risk register first
**Present when the PRD has solid evidence but many open questions about feasibility, scope, or technical unknowns.**

Look for: Open Questions table with 4+ items, feasibility risks marked High in the V/U/F/V section, dependency list with "External" items not yet confirmed, or technical uncertainty phrases like "TBD," "needs research," "unclear how to implement."

**Route: pm-risk-register before pm-tech-spec**

Present this at the gate:

```
---
ROTEAMENTO — pm-prd concluído

O PRD tem boa evidência do problema, mas apresenta alta incerteza técnica:
[N] perguntas em aberto + riscos de viabilidade sinalizados.

Avançar direto para a spec pode resultar em uma arquitetura que ignora
os maiores bloqueadores — descobertos só quando a engenharia já está no meio do trabalho.

Caminho recomendado:
→ pm-risk-register — documentar e mitigar riscos antes de comprometer com arquitetura
→ pm-tech-spec — especificar com os riscos já mapeados e tratados

O que você prefere?
— "Sim, avaliar riscos primeiro" → rodar pm-risk-register → depois pm-tech-spec
— "Avançar direto para a spec" → continuar para pm-tech-spec sem risk register
— "Parar aqui" para encerrar
---
```

---

### Signal 3: Evidence is solid — proceed normally

**Route: pm-tech-spec (standard path)**

Present this at the gate:

```
---
ROTEAMENTO — pm-prd concluído

O PRD está bem fundamentado. Os próximos passos seguem o fluxo padrão.

Próxima skill: pm-tech-spec
O que passa: problema, critérios de sucesso e escopo do PRD

Aprovado para avançar?
— "Aprovado" para chamar pm-tech-spec
— "Ajustar: [o que mudar]" para corrigir antes de avançar
— "Parar aqui" para encerrar
---
```

---

## Context handoff between skills

| From | To | What passes forward |
|------|----|---------------------|
| `pm-prd` | `pm-tech-spec` | Problem statement, success criteria, scope |
| `pm-prd` | `pm-risk-register` | Initiative scope, assumptions, open questions |
| `pm-risk-register` | `pm-tech-spec` | Mitigated risks, confirmed constraints, architectural guard-rails |
| `pm-tech-spec` | `pm-user-stories` | Architecture constraints, API design, edge cases |
| `pm-user-stories` | `pm-sprint-planning` | Story list with effort estimates |

State what you're carrying forward:
> "I'm using the PRD's success criteria and scope boundaries to write the tech spec. You won't need to re-explain the problem."

---

## Full execution cycle sequences

**Evidence solid (standard path):**
```
pm-prd → [CONDITIONAL GATE: solid] → pm-tech-spec → [GATE] → pm-user-stories → [GATE] → pm-sprint-planning
```

**High uncertainty path:**
```
pm-prd → [CONDITIONAL GATE: high uncertainty] → pm-risk-register → [GATE] → pm-tech-spec → [GATE] → pm-user-stories → [GATE] → pm-sprint-planning
```

**Weak evidence path (redirect):**
```
pm-prd → [CONDITIONAL GATE: weak evidence] → [pause → pm-discovery-orchestrator] → return with validated PRD → pm-tech-spec
```

---

## Bridge to launch

After user stories are approved:
> "The feature is specced and ready to build. When it's close to shipping, `pm-launch-orchestrator` will help you prepare the GTM, onboarding, and success metrics for the launch."

---

## Output files (PT-BR)

```
execution/outputs/
  prd-[feature]-[YYYY-MM-DD].md
  tech-spec-[feature]-[YYYY-MM-DD].md
  user-stories-[feature]-[YYYY-MM-DD].md
  sprint-plan-[YYYY-MM-DD].md
  registro-riscos-[YYYY-MM-DD].md
```
