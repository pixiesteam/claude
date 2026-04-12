---
name: pm-launch-orchestrator
description: >
  Orchestrates product and feature launches — from go-to-market strategy to onboarding flow to success metrics dashboard to pre-launch stress testing. Use whenever a PM is preparing to launch a product or major feature, needs a GTM plan, is redesigning activation for a launch, or wants to stress-test launch readiness. Also triggers for: "lançamento de produto", "preparar lançamento", "gtm", "ativar usuários", "onboarding para lançamento", "métricas de lançamento", "estamos prontos para lançar". This orchestrator calls independent PM skills in sequence — each skill must be installed separately.
---

# PM Launch Orchestrator

Guides the PM through launch preparation: from go-to-market strategy to onboarding design to success measurement to launch readiness review.

**Language:** All conversation in English. All output documents in Portuguese (Brazil).

---

## Required skills

| Skill name | Step | When |
|------------|------|------|
| `pm-go-to-market` | 1 | Define segments, channels, messaging, timeline |
| `pm-onboarding-flow-designer` | 2 | Design the activation path for new users |
| `pm-metrics-dashboard` | 3 | Set up launch success measurement |
| `pm-devil-advocate` | 4 | Stress-test launch readiness — **this gate has conditional outcomes** |

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
context/product.md       → what's launching, current activation metrics
context/personas.md      → target users, aha moment
context/company.md       → sales motion, channels, team
context/competitors.md   → competitive positioning
execution/outputs/prd-*  → PRD for the feature launching
strategy/outputs/        → positioning or pricing outputs
```

State what you found, then ask: **"What's the launch timeline, and what's the biggest concern going into it?"**

**Entry A — Full launch preparation (6+ weeks out)**
> Full cycle: GTM → Onboarding → Dashboard → Devil's Advocate.

**Entry B — GTM only (launch is defined, need the plan)**
> Start directly with `pm-go-to-market`.

**Entry C — Activation problem at launch**
> Start with `pm-onboarding-flow-designer`.

**Entry D — Launch is close, need a readiness check**
> Start directly with `pm-devil-advocate` — challenge the launch plan before it goes live.

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

### Standard gate prompt (use after GTM, onboarding, and dashboard outputs)

```
---
REVISÃO NECESSÁRIA — [Nome da Skill Concluída]

Antes de avançar:
✓ O conteúdo reflete o plano de lançamento que você tem em mente?
✓ Há segmentos, canais ou riscos que ficaram de fora?
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

## CONDITIONAL ROUTING — after pm-devil-advocate

The Devil's Advocate is the final gate before launch. Unlike every other gate in this system, approval here does **not** automatically mean "proceed." Instead, read the overall assessment from the Devil's Advocate output and route based on the risk verdict.

### Signal 1: 🔴 Alto Risco — pause launch
**Present when the Devil's Advocate output shows critical unresolved risks, fundamental assumptions that are invalidated, or scenarios where launching would likely cause significant customer harm, trust damage, or reputational impact.**

Look for: overall assessment marked 🔴, "A Pergunta Mais Crítica" that exposes a foundational gap, failure scenarios with High probability AND High impact, or blind spots in the "Não Abordado no Documento" section that affect the core launch premise.

**Route: do not launch — create a resolution plan**

Present this at the gate:

```
---
DECISÃO DE LANÇAMENTO — pm-devil-advocate: 🔴 ALTO RISCO

O stress-test identificou riscos críticos que precisam ser resolvidos
antes do lançamento. Lançar agora exporia o produto a [resumir o risco principal].

Opções:
— "Endereçar riscos — voltar para execução" → pausar lançamento, criar plano de
  mitigação para os riscos 🔴, resolver, e re-rodar pm-devil-advocate antes de lançar
— "Endereçar riscos — voltar para discovery" → os riscos revelam que o problema
  não está bem entendido; rodar pm-discovery-orchestrator para revalidar
— "Lançar mesmo assim (assumindo o risco)" → registrar os riscos explicitamente
  e lançar com os olhos abertos. Use apenas se o custo de atrasar supera
  claramente o custo do risco.
---
```

**If the PM chooses to launch anyway:** add this block to the top of the launch readiness checklist:
> "⚠️ LANÇAMENTO COM RISCO ASSUMIDO: Os seguintes riscos críticos foram identificados e conscientemente aceitos: [listar]. Monitorar ativamente nos primeiros 7 dias pós-lançamento."

---

### Signal 2: 🟡 Em Risco — conditional launch
**Present when risks are real but manageable — there are specific actions that, if completed before launch, would reduce the risk to an acceptable level.**

Look for: overall assessment marked 🟡, risks that have clear mitigations already identified in the Devil's Advocate output, "Antes de avançar" section with concrete and achievable actions, and no single failure scenario with both High probability AND High impact simultaneously.

**Route: conditional launch with mandatory checklist**

Present this at the gate:

```
---
DECISÃO DE LANÇAMENTO — pm-devil-advocate: 🟡 EM RISCO

O lançamento pode prosseguir condicionado à conclusão das mitigações abaixo.
Não lançar sem confirmar cada item.

Checklist de mitigação obrigatória antes de lançar:
- [ ] [Mitigação 1 — extraída do Devil's Advocate]
- [ ] [Mitigação 2]
- [ ] [Mitigação 3]

Após completar todos os itens:
— "Mitigações concluídas — pronto para lançar" → avançar para readiness checklist final
— "Não consigo concluir [item X]" → reavalie se o lançamento deve ser adiado
---
```

---

### Signal 3: 🟢 Sólido — approved to launch
**Present when the Devil's Advocate output shows minor or well-mitigated risks, validated assumptions, and no critical blind spots.**

Look for: overall assessment marked 🟢, stress tests that the plan survives, risks rated as Low probability or Low impact, and the "O Desafio Final" question that has already been answered in the plan.

**Route: approved to launch — proceed to readiness checklist**

Present this at the gate:

```
---
DECISÃO DE LANÇAMENTO — pm-devil-advocate: 🟢 SÓLIDO

O plano de lançamento passou no stress-test. Os riscos identificados são
gerenciáveis e as principais suposições estão validadas.

Aprovado para lançar. Próximo passo: confirmar o launch readiness checklist abaixo.

LAUNCH READINESS CHECKLIST FINAL:
- [ ] GTM plan aprovado com owners atribuídos
- [ ] Onboarding flow testado com 3–5 usuários reais
- [ ] Dashboard configurado e mostrando dados
- [ ] Time de vendas treinado (se motion sales-led)
- [ ] Time de CS briefado sobre a nova feature
- [ ] Rollback plan definido (o que aciona, quem executa)
- [ ] Comunicação de lançamento revisada e agendada

Quando todos os itens estiverem completos: lançar.
---
```

---

## Context handoff between skills

| From | To | What passes forward |
|------|----|---------------------|
| `pm-go-to-market` | `pm-onboarding-flow-designer` | Primary persona, aha moment, activation goal |
| `pm-go-to-market` | `pm-metrics-dashboard` | 30/60/90-day success metrics |
| `pm-onboarding-flow-designer` | `pm-metrics-dashboard` | Step-level activation metrics to track |
| Any | `pm-devil-advocate` | The full launch plan as input for stress testing |

State what you're carrying forward:
> "I'm using the primary persona and aha moment from the GTM plan to design the onboarding flow. You won't need to redefine who we're designing for."

---

## Full launch preparation sequences

**Standard path (full cycle):**
```
pm-go-to-market → [GATE] → pm-onboarding-flow-designer → [GATE] → pm-metrics-dashboard → [GATE] → pm-devil-advocate → [CONDITIONAL GATE]
```

**From conditional gate — green:**
```
pm-devil-advocate → [🟢 Sólido] → launch readiness checklist → LAUNCH
```

**From conditional gate — yellow:**
```
pm-devil-advocate → [🟡 Em Risco] → mitigation checklist → confirm → LAUNCH
```

**From conditional gate — red:**
```
pm-devil-advocate → [🔴 Alto Risco] → [back to execution OR discovery] → re-run devil-advocate → LAUNCH
```

---

## Bridge to execution

If the PRD and spec aren't done yet:
> "Before planning the launch, the feature needs a PRD and tech spec. `pm-execution-orchestrator` handles that cycle — come back here when the feature is close to shipping."

---

## Output files (PT-BR)

```
strategy/outputs/
  gtm-[produto]-[YYYY-MM-DD].md
discovery/outputs/
  fluxo-onboarding-[YYYY-MM-DD].md
strategy/outputs/
  spec-dashboard-[YYYY-MM-DD].md
communication/outputs/
  revisao-critica-launch-[YYYY-MM-DD].md
```
