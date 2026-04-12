---
name: pm-discovery-orchestrator
description: >
  Orchestrates the full discovery cycle — understanding users, opportunities, and evidence before deciding what to build. Use whenever a PM mentions discovery, user research, customer insights, understanding user needs, analyzing feedback, finding opportunities, activation problems, funnel issues, or anything about users. Also triggers for: "fazer discovery", "entender o usuário", "analisar feedback", "o que os usuários precisam", "oportunidades de produto", "problema de ativação", "drop-off no funil". This orchestrator calls independent PM skills in sequence — each skill must be installed separately.
---

# PM Discovery Orchestrator

Guides the PM through discovery: from raw signals to structured opportunities, validated jobs, and testable solutions.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | Mode | When |
|------------|------|------|
| `pm-interview-synthesizer` | Research synthesis | You have raw interview transcripts or notes |
| `pm-app-review-analyzer` | Research synthesis | You have review data |
| `pm-feature-request-analyzer` | Research synthesis | You have requests from multiple sources |
| `pm-funnel-analyzer` | Research synthesis | You have conversion/drop-off data |
| `pm-jtbd-extractor` | Opportunity structuring | You have research; need underlying jobs |
| `pm-value-prop-canvas` | Opportunity structuring | You need to map user needs vs. product fit |
| `pm-opportunity-solution-tree` | Opportunity structuring | You have an outcome; need to explore solutions |
| `pm-onboarding-flow-designer` | Experience design | Activation is the problem |

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
context/product.md     → activation rate, known issues, current metrics
context/personas.md    → user segments, existing JTBD, pain points
context/company.md     → strategic priorities
discovery/outputs/     → previous discovery outputs
```

Then identify the entry point with one question: **"What do you have, and what are you trying to figure out?"**

| Entry | Situation | First skill |
|-------|-----------|-------------|
| A | Raw interview notes to synthesize | `pm-interview-synthesizer` |
| A2 | Raw quantitative data to understand | `pm-app-review-analyzer` or `pm-feature-request-analyzer` or `pm-funnel-analyzer` |
| B | Research done, needs structuring | `pm-jtbd-extractor` or `pm-value-prop-canvas` |
| C | Outcome defined, need solutions | `pm-opportunity-solution-tree` |
| D | Activation problem specifically | `pm-funnel-analyzer` → `pm-onboarding-flow-designer` |

State the entry point before calling any skill:
> "Looks like Entry B — you have research and need to structure it into jobs. I'll call `pm-jtbd-extractor` next."

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
✓ O conteúdo reflete o que você está vendo nos dados/pesquisa?
✓ Há temas, padrões ou oportunidades que ficaram de fora?
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

## CONDITIONAL ROUTING — after pm-jtbd-extractor

After delivering the JTBD output and the PM approves it, **do not automatically proceed to the next skill**. Instead, read the content of the approved JTBD output and evaluate which of these three signals is most present:

### Signal 1: Activation problem
**Present when the jobs reveal that users arrive but can't reach value** — the product may be right, but the path to the aha moment is broken.

Look for: high importance jobs around "getting started," "understanding what to do first," "seeing value quickly," satisfaction scores that are low specifically on early-stage jobs, explicit mentions of onboarding friction, or context from `context/product.md` showing activation rate below target.

**Route: pm-funnel-analyzer → pm-onboarding-flow-designer**

Present this at the gate:

```
---
ROTEAMENTO — pm-jtbd-extractor concluído

Com base nos jobs identificados, os dados apontam para um problema de ativação:
os usuários têm o job de [job principal], mas a satisfação é baixa nas etapas iniciais —
sugerindo que o produto não está entregando valor rápido o suficiente.

Caminho recomendado:
→ pm-funnel-analyzer — mapear onde os usuários estão saindo antes de chegar ao valor
→ pm-onboarding-flow-designer — redesenhar o caminho até o aha moment

Você concorda com essa leitura?
— "Sim, problema de ativação" para seguir esse caminho
— "Não, o problema é diferente: [descreva]" para ajustar o diagnóstico
— "Parar aqui" para encerrar
---
```

---

### Signal 2: Opportunity gap — move to solution space
**Present when the jobs are clear and well-validated, with no activation issues — the problem is understood, and it's time to explore solutions.**

Look for: high importance + low satisfaction jobs that are clearly scoped, no onboarding-related jobs in the top tier, research that points to a specific friction in a known feature, or context from `context/product.md` showing that activation is healthy but engagement drops mid-journey.

**Route: pm-opportunity-solution-tree**

Present this at the gate:

```
---
ROTEAMENTO — pm-jtbd-extractor concluído

Os jobs estão bem definidos e apontam para uma oportunidade clara de melhoria
em [área específica]. A ativação não é o problema — é um gap de valor no produto existente.

Próxima skill: pm-opportunity-solution-tree
O que passa: os jobs de alta importância e baixa satisfação como outcome central

Aprovado para avançar?
— "Aprovado" para chamar pm-opportunity-solution-tree
— "Ajustar: [o que mudar]" para corrigir antes de avançar
— "Parar aqui" para encerrar
---
```

---

### Signal 3: Insufficient evidence — more research needed
**Present when the jobs are hypothetical, patterns are weak, or the data sample is too small to act on.**

Look for: majority of jobs marked "⚠️ hipótese — validar", fewer than 5 data points per job category, conflicting signals between what users say and what analytics show, or explicit "not enough data" notes in the synthesis.

**Route: back to research synthesis skills**

Present this at the gate:

```
---
ROTEAMENTO — pm-jtbd-extractor concluído

Os jobs identificados são ainda muito hipotéticos para guiar decisões de produto com confiança.
Construir soluções agora seria apostar em padrões que podem não se confirmar.

Recomendação: mais pesquisa antes de avançar para soluções.

O que você quer fazer?
— "Coletar mais dados — entrevistas" → rodar pm-interview-synthesizer com novos dados
— "Coletar mais dados — quantitativos" → rodar pm-funnel-analyzer ou pm-app-review-analyzer
— "Avançar mesmo assim" → continuar para pm-opportunity-solution-tree, marcando outputs como hipotéticos
— "Parar aqui" para encerrar
---
```

---

## Context handoff between skills

State what you're carrying forward:
> "I'm using the top jobs from the JTBD output as the outcome anchors for the OST. You won't need to re-explain the user needs."

| From | To | What passes forward |
|------|----|---------------------|
| `pm-interview-synthesizer` | `pm-jtbd-extractor` | Themes, pain points, verbatim quotes |
| `pm-app-review-analyzer` | `pm-jtbd-extractor` | Patterns, frequency data, verbatim reviews |
| `pm-feature-request-analyzer` | `pm-jtbd-extractor` | Clustered requests, underlying needs |
| `pm-funnel-analyzer` | `pm-onboarding-flow-designer` | Drop-off points, step-level conversion data |
| `pm-jtbd-extractor` | `pm-opportunity-solution-tree` | High importance / low satisfaction jobs |
| `pm-jtbd-extractor` | `pm-value-prop-canvas` | User segments and their primary jobs |

---

## Full discovery cycle sequences

**From raw research:**
```
pm-interview-synthesizer → [GATE] → pm-jtbd-extractor → [CONDITIONAL GATE] → pm-opportunity-solution-tree
```

**Activation problem path:**
```
pm-funnel-analyzer → [GATE] → pm-onboarding-flow-designer
```

**Full cycle (cold start):**
```
pm-interview-synthesizer → [GATE] → pm-jtbd-extractor → [CONDITIONAL GATE] →
  → pm-opportunity-solution-tree OR
  → pm-funnel-analyzer → pm-onboarding-flow-designer
```

---

## Bridge to execution

After discovery is complete:
> "You now have validated jobs and mapped opportunities. These are the inputs for `pm-execution-orchestrator` — the JTBD and OST outputs feed directly into a grounded PRD."

---

## Output files (PT-BR)

```
discovery/outputs/
  sintese-entrevistas-[YYYY-MM-DD].md
  analise-reviews-[YYYY-MM-DD].md
  analise-solicitacoes-[YYYY-MM-DD].md
  analise-funil-[YYYY-MM-DD].md
  jtbd-[YYYY-MM-DD].md
  arvore-oportunidades-[YYYY-MM-DD].md
  fluxo-onboarding-[YYYY-MM-DD].md
```
