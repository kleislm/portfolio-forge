# Innovation Playbook — H1 / H2 / H3, AI governance, responsible AI

🇧🇷 Português · [🇬🇧 English ↓](#-english)

> Como organizo portfólio de inovação, governança de IA e mudança organizacional.
> Não é teoria — é o que apliquei como founder e como advisor.

## 1. Portfólio em três horizontes (McKinsey 3 Horizons, recalibrado para IA)

Cada aposta de inovação se encaixa em um horizonte. Os três coexistem; nenhum substitui o outro.

| Horizonte | Tese | Métrica primária | Risco | Cases meus |
|---|---|---|---|---|
| **H1** — IA assistiva no core | Otimização do fluxo atual. Não reinventa, acelera. ROI claro em 1–2 trimestres | Tempo economizado / produtividade | Baixo (entrega ou volta) | Assist4Doc, Agenda Connect |
| **H2** — Novo *operating model* | Reorganização do trabalho com IA. Muda o "como". Frequentemente exige *change management* | Conversão de % das tarefas para o novo modelo | Médio-alto | Lastro |
| **H3** — Opção estratégica (plataforma/OSS/parceria) | Aposta de baixa probabilidade, alto impacto. Constrói opcionalidade | *Optionality* — número de portas abertas | Alto (mas o downside é limitado ao tempo investido) | NoKey, Staff Forge |

**Regra:** ~70% do budget em H1, ~20% em H2, ~10% em H3. Quando uma organização tem 100% em H1, ela está aposentada. Quando tem 100% em H3, ela quebra.

## 2. Mapa de decisão "construir vs. comprar vs. parceiro vs. esperar"

| Sinal | Construir | Comprar | Parceiro | Esperar |
|---|:--:|:--:|:--:|:--:|
| É vantagem competitiva direta? | ✅ | ❌ | parcial | ❌ |
| Há fornecedor maduro e barato? | ❌ | ✅ | parcial | depende |
| Dado é sensível/regulado? | ✅ | ⚠️ | ⚠️ | — |
| Tempo crítico (janela < 6m)? | ⚠️ | ✅ | ✅ | ❌ |
| Tecnologia ainda imatura (drift mensal)? | ⚠️ | ⚠️ | ⚠️ | ✅ |
| Custo opcional do erro é baixo? | — | ✅ | — | — |

**Anti-padrão recorrente:** organização constrói o que devia comprar (CRM, autenticação, eval framework genérico) e compra o que devia construir (IA vertical no core do negócio). Inverter é alavanca enorme.

## 3. Governança de IA — IA Responsável operacional

Não é PowerPoint. É **5 controles que rodam por trás de cada *release*** de IA.

### Controle 1 — Inventário de IA
Toda integração com modelo (próprio ou terceiro) entra num inventário com: finalidade, base legal LGPD, dados tratados, tier de risco (baixo/médio/alto/inaceitável — referência: AI Act EU + ANPD), responsável técnico, fornecedor.

### Controle 2 — RIPD / DPIA para IA de alto risco
Decisão clínica, jurídica, de crédito ou de seleção de pessoas → **DPIA obrigatório** (LGPD art. 38). Documenta finalidade, necessidade, proporcionalidade, base legal, risco residual.

### Controle 3 — Human-in-the-loop sempre que a decisão tem custo
Não é compliance — é *design choice*. **A IA propõe; o humano decide.** Em Assist4Doc, médico assina. Em Lastro, advogado revisa. O sistema **não consegue** entregar resultado sem o gate humano.

### Controle 4 — Eval + verificador adversarial em produção
Detalhe em [`engineering/eval-methodology.md`](../engineering/eval-methodology.md). Sem isso, "IA responsável" é declaração de intenção.

### Controle 5 — Plano de resposta a incidente (LGPD art. 48)
Vazamento ou alucinação grave → comunicação à ANPD em 48h se houver dado pessoal. Runbook treinado. Já apliquei em política real (agente `backup-escritorio`).

## 4. Change management — como aceito ou recuso uma adoção de IA no time

Sequência de discovery interna antes de adotar feature de IA pesada:

1. **Qual é a tarefa-âncora?** (não "vamos usar IA", mas "vamos reduzir tempo de X de Y para Z")
2. **Quem é o early adopter natural?** (a pessoa que já reclama da tarefa)
3. **Qual é o *fall-back* quando falhar?** (porque vai falhar — semana 2 ou 3)
4. **Métrica honesta:** `% tarefas que voltam para o modo antigo após 30 dias`
5. **Gate de continuar:** % > 30 → roll back; % < 10 → expandir; meio → iterar prompt/dataset

## 5. Antipadrões que vi de perto (e como evito)

| Antipadrão | Como evito |
|---|---|
| "Vamos colocar um chatbot" sem caso de uso | Recuso. Exijo problema mensurável primeiro |
| Eval virou checkbox depois do *go-live* | Eval **no CI**, score gate trava merge |
| RAG sem verificador, todo mundo confia na "fonte" | Adversarial verifier obrigatório em domínio crítico |
| Modelo X economiza 60% do custo, vamos trocar | Não troca sem rerun do eval suite |
| Comitê de IA Responsável mensal sem operação por trás | Comitê só tem reunião com 5 controles vivos |
| LGPD entra no fim do projeto | LGPD/CFM/Bacen na inception, no spec |
| "Confidencial, então não posso testar com modelo externo" | Avaliação com Ollama local + dados sintéticos antes de subir para terceiro |

## 6. Tese de carreira por trás disso

Inovação em IA precisa de gente que **fecha o loop**. Quem só conduz reunião não enxerga onde o ROI evapora; quem só programa não enxerga horizonte 2/3. Eu fico no meio — e codo quando precisa.

---

## 🇬🇧 English

> How I organize an innovation portfolio, AI governance, and organizational change.
> Not theory — what I've applied as founder and advisor.

## 1. Three-horizon portfolio (McKinsey 3 Horizons, recalibrated for AI)

Every innovation bet fits one horizon. All three coexist; none replaces the others.

| Horizon | Thesis | Primary metric | Risk | My cases |
|---|---|---|---|---|
| **H1** — Assistive AI on the core | Optimizes today's flow. Doesn't reinvent, accelerates. Clear ROI in 1–2 quarters | Time saved / productivity | Low (ships or rolls back) | Assist4Doc, Agenda Connect |
| **H2** — New operating model | AI reorganizes the work itself. Often requires change management | % of tasks converted to the new model | Medium–high | Lastro |
| **H3** — Strategic option (platform/OSS/partnership) | Low-probability, high-impact bet. Builds optionality | Optionality — open doors | High (but downside is limited to invested time) | NoKey, Staff Forge |

**Rule:** ~70% of budget in H1, ~20% in H2, ~10% in H3. 100% in H1 = retired. 100% in H3 = bankrupt.

## 2. Decision map "build vs. buy vs. partner vs. wait"

| Signal | Build | Buy | Partner | Wait |
|---|:--:|:--:|:--:|:--:|
| Is it direct competitive advantage? | ✅ | ❌ | partial | ❌ |
| Mature, cheap vendor available? | ❌ | ✅ | partial | depends |
| Data is sensitive/regulated? | ✅ | ⚠️ | ⚠️ | — |
| Time-critical (< 6m window)? | ⚠️ | ✅ | ✅ | ❌ |
| Tech still immature (monthly drift)? | ⚠️ | ⚠️ | ⚠️ | ✅ |
| Low optional cost of error? | — | ✅ | — | — |

**Recurring anti-pattern:** orgs build what they should buy (CRM, auth, generic eval frameworks) and buy what they should build (vertical AI at the core of the business). Flipping it is a huge lever.

## 3. AI Governance — operational Responsible AI

Not PowerPoint. **5 controls that run behind every AI release.**

### Control 1 — AI inventory
Every model integration (own or third-party) enters an inventory: purpose, legal basis (LGPD), data processed, risk tier (low/medium/high/unacceptable — ref. EU AI Act + ANPD), technical owner, vendor.

### Control 2 — DPIA / RIPD for high-risk AI
Clinical, legal, credit, or hiring decisions → **mandatory DPIA** (LGPD art. 38). Documents purpose, necessity, proportionality, legal basis, residual risk.

### Control 3 — Human-in-the-loop whenever the decision has cost
Not compliance — design choice. **AI proposes; the human decides.** In Assist4Doc, the physician signs. In Lastro, the lawyer reviews. The system **cannot** ship without the human gate.

### Control 4 — Production evals + adversarial verifier
Detail in [`engineering/eval-methodology.md`](../engineering/eval-methodology.md). Without this, "Responsible AI" is a statement of intent.

### Control 5 — Incident response plan (LGPD art. 48)
Leak or serious hallucination → ANPD notification in 48h if personal data is involved. Trained runbook. I've already applied this in actual policy (see backup-firm agent).

## 4. Change management — how I accept or reject an AI adoption in the team

Internal discovery sequence before adopting a heavy AI feature:

1. **What's the anchor task?** (not "let's use AI", but "let's reduce time of X from Y to Z")
2. **Who's the natural early adopter?** (the person already complaining about the task)
3. **What's the fall-back when it fails?** (because it will — week 2 or 3)
4. **Honest metric:** `% of tasks reverting to the old way after 30 days`
5. **Continue gate:** % > 30 → roll back; % < 10 → expand; middle → iterate prompt/dataset

## 5. Anti-patterns I've seen (and how I avoid them)

| Anti-pattern | How I avoid it |
|---|---|
| "Let's put a chatbot" without a use case | I refuse. I demand a measurable problem first |
| Eval became a post-launch checkbox | Eval **in CI**, score gate blocks merge |
| Hallucination-blind RAG, everyone trusts "the source" | Mandatory adversarial verifier in critical domains |
| Model X saves 60% cost, let's swap | No swap without rerunning the eval suite |
| Monthly "Responsible AI committee" with no operation behind | Committee only meets with 5 live controls |
| LGPD comes at the end of the project | LGPD/CFM/Bacen at inception, in the spec |
| "Confidential, so I can't test with an external model" | Eval with local Ollama + synthetic data before going to a vendor |

## 6. Career thesis behind this

AI innovation needs people who **close the loop**. Whoever only runs meetings can't see where ROI evaporates; whoever only codes can't see horizon 2/3. I sit in the middle — and I code when needed.
