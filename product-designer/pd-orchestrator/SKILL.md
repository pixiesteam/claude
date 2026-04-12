---
name: pd-orchestrator
description: >
  Orchestrate the Product Designer skill pipeline. Use this skill whenever a product designer (or
  someone working in product design) needs help with any part of their work — research, strategy,
  artifacts, facilitation, or stakeholder communication. Triggers on phrases like "preciso de ajuda
  com design", "cria um roteiro de pesquisa", "quero mapear um fluxo", "preciso apresentar essa
  decisão", "vamos fazer um discovery", "como priorizo isso", "me ajuda a estruturar uma critique",
  "cria uma persona", "quero especificar esse wireframe", or any design-related task.
---

# Product Designer Orchestrator

Ponto de entrada do pipeline de Product Design. Garante que o contexto do projeto está completo
antes de rotear para qualquer agente.

**Language:** All content and conversation in PT-BR.

---

## Como o contexto funciona

Todos os agentes deste pipeline leem dois arquivos antes de produzir qualquer output:

- **`pd-context.md`** — contexto permanente do projeto (produto, usuário, DS, stack, fase)
- **`pd-brief.md`** — briefing da tarefa atual (o que o designer precisa agora)

O orquestrador é responsável por garantir que esses arquivos existem e estão completos
**antes** de chamar qualquer agente. Os agentes não tomam decisões sobre contexto faltando —
eles recebem o contexto pronto.

---

## Step 0 — Montar pd-context.md (SEMPRE PRIMEIRO)

### Se pd-context.md não existe

Faça uma entrevista em PT-BR em uma única mensagem:

> "Antes de começar, preciso entender o projeto. Me conta:
>
> 1. **Produto** — O que é? Em uma frase, qual problema resolve e para quem?
> 2. **Usuário** — Quem usa? Perfil principal, o que quer realizar, o que frustra hoje?
> 3. **Fase** — Em que fase está? (Discovery / Definição / Execução / Lançamento)
> 4. **Design System** — Usa algum DS? Se sim, qual? O arquivo `ds-foundation.md` existe?
> 5. **Stack** — Qual framework/tecnologia? (React, Vue, Vanilla, mobile...)
> 6. **Restrições** — Alguma limitação importante? (prazo, plataforma, negócio)"

Depois da entrevista, gere e salve `pd-context.md`:

```markdown
# Contexto do Projeto

**Última atualização:** [data] — Orchestrator (setup inicial)

---

## Produto

[Uma frase: o que é e qual problema resolve para quem.]

## Usuário principal

**Perfil:** [quem é, o que faz, contexto de uso]
**Objetivo:** [o que quer realizar]
**Dores:** [o que frustra ou bloqueia hoje]

## Personas detalhadas

[Se context/personas.md existe: "Ver context/personas.md"
 Se não existe: resumo do que foi dito na entrevista, ou "não definidas"]

## Fase atual

[Discovery / Definição / Execução / Lançamento]

## Design System

**Nome:** [nome do DS, ou "não definido"]
**ds-foundation.md:** [existe / não existe]

## Stack e framework

[Ex: React 18 + Next.js 14 App Router / Vue 3 / Vanilla HTML]

## Restrições conhecidas

- [restrição 1]
- [ou "nenhuma identificada"]

---

## Histórico

| Data | Por quem | O que mudou |
|------|----------|-------------|
| [data] | Orchestrator | Setup inicial |
```

Apresente via `present_files`. Aplique gate protocol. Só avance com aprovação.

---

### Se pd-context.md existe

Carregue silenciosamente. Verifique dois campos específicos:

**1. Campo "ds-foundation.md: não existe"**
Se o pipeline vai usar `pd-ui-specs-agent`, o ds-foundation.md é necessário.
Pergunte agora, antes de rotear:
> "Para especificar componentes com tokens reais, preciso do ds-foundation.md.
> Ele existe? Se sim, pode indicar o caminho. Se não, quer criar agora com o DS Foundation Agent?"
- Existe → registre o caminho no pd-context.md e continue
- Não existe, quer criar → rode o DS Foundation Agent antes de continuar
- Não existe, não quer criar agora → continue, mas o specs agent vai usar descrições genéricas

**2. Campo "Personas: não definidas"**
Se o pipeline vai usar pd-ux-artifacts-agent, pd-research-agent ou pd-strategy-agent,
personas enriquecem o output. Pergunte uma vez:
> "Tem personas ou JTBD documentados? Se o PM criou um context/personas.md, posso usá-lo.
> Se não, os agentes vão trabalhar com o que está no pd-context.md."
- Existe → registre no pd-context.md e passe para os agentes
- Não existe → continue normalmente

Mostre uma linha de confirmação:
> "Contexto carregado: [nome do produto], fase [fase]. O que você precisa hoje?"

---

### Se o usuário pede para atualizar o contexto

Frases: "atualizar contexto", "o produto mudou", "nova fase", "mudar projeto"

Mostre o pd-context.md atual. Pergunte o que mudou. Atualize só os campos mencionados.
Incremente a versão no histórico.

---

## Step 1 — Montar pd-brief.md (a cada nova tarefa)

Com pd-context.md confirmado, identifique o que o usuário precisa agora.
Se não estiver claro, faça **no máximo 2 perguntas**.

Gere e salve `pd-brief.md` (sobrescreve a cada tarefa — não é histórico):

```markdown
# Brief da Tarefa

**Data:** [data]
**Fase:** [Discovery / Definição / Execução / Lançamento / Facilitação / Comunicação]
**Agente:** [nome do agente que vai rodar]

---

## O que o designer precisa

[Uma frase objetiva: output desejado]

## Contexto específico desta tarefa

[Informação relevante apenas agora — que não pertence ao pd-context.md permanente]

## Restrições desta tarefa

[Prazo, audiência, formato — ou "nenhuma além do pd-context.md"]
```

---

## Step 2 — Rotear para o agente

Com `pd-context.md` e `pd-brief.md` prontos, chame o agente correto.
Passe ambos os arquivos. O agente não precisa perguntar nada que já está neles.

| Fase | Agente |
|------|--------|
| Discovery | `pd-research-agent` |
| Definição | `pd-strategy-agent` |
| Ideação / UX | `pd-ux-artifacts-agent` |
| Execução / UI | `pd-ui-specs-agent` |
| Facilitação | `pd-facilitation-agent` |
| Comunicação | `pd-communication-agent` |

Se a necessidade cobrir mais de uma fase, acione em sequência com gate entre cada agente.

---

## MANDATORY GATE PROTOCOL

Every agent runs inside this loop — no exceptions:

```
1. Run the agent → deliver its output
2. STOP completely
3. Present the gate prompt below
4. Wait for explicit approval
5. Only then run the next agent
```

**NEVER run two agents in the same response.**
**NEVER interpret enthusiasm as approval.**
**NEVER suggest the next agent inside an agent's output.**

### Gate prompt

```
---
REVISÃO NECESSÁRIA — [Nome do Agente]

✓ O artefato reflete o que você tem em mente?
✓ Há algo que precisa ser ajustado?
✓ Campos com [VALIDAR COM PESQUISA] estão corretos?

— "Aprovado" para avançar
— "Ajustar: [o que mudar]"
— "Parar aqui"
---
```

### Se o agente revelou informação nova sobre o projeto

Adicione ao gate:
> "O agente revelou [informação]. Quer que eu atualize o pd-context.md com isso?"
- Sim → atualiza antes de avançar
- Não → registra só no artefato

---

## Iteration limit

Cada agente tem máximo de **3 rodadas de ajuste**.
Após a 3ª:
> "Já fizemos 3 rodadas aqui. Quer continuar refinando, recomeçar do zero, ou seguir com a versão atual?"

---

## Sequências comuns

| Objetivo | Sequência |
|----------|-----------|
| Discovery completo | pd-research-agent → [GATE] → pd-strategy-agent |
| Apresentar pesquisa | pd-research-agent → [GATE] → pd-communication-agent |
| Especificar feature | pd-ux-artifacts-agent → [GATE] → pd-ui-specs-agent |
| Design review | pd-ui-specs-agent → [GATE] → pd-communication-agent |
