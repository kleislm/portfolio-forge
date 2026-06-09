# Eval methodology — multi-AI panel, LLM-as-judge, regression suite

🇧🇷 Português · [🇬🇧 English ↓](#-english)

> "Feature de IA sem eval é aposta." — minha tese 2.
> Tudo que entra em produção passa por um pipeline de eval com três camadas: **offline** (dataset curado), **online** (métricas reais), **adversarial** (verificador externo) — e tem orçamento de custo/latência registrado.

## Princípios

1. **Curado, não automático.** Dataset de eval é montado à mão a partir de casos reais — não geramos sintético no dia 1.
2. **Múltiplos juízes, não um.** Um único LLM-as-judge é vulnerável a *self-preference*. Painel de 3–5 modelos independentes vota.
3. **Fail-closed.** Se < maioria aprovar, o sistema **para** e devolve para humano. Não tenta entregar resultado duvidoso.
4. **Métrica conectada a valor.** "Acurácia ROUGE" não diz nada para clínico/jurídico. Quero `taxa-de-revisão-zero-emenda`, `tempo-economizado-por-tarefa`, `cost-per-resolved-task`.
5. **Regressão sempre.** Eval roda em CI a cada PR. *Score* abaixo do baseline trava o merge.

## Pipeline em 4 camadas

### Camada 1 — Eval offline (dataset curado)

| Item | Detalhe |
|---|---|
| **Tamanho** | 50–200 casos por tipo de tarefa (peticionamento, SOAP, classificação) |
| **Origem** | 100% casos reais anonimizados — não sintéticos no MVP |
| **Anotação** | Resposta "ouro" + critério de aceite (rubrica) por caso |
| **Métrica** | Score 0–5 por rubrica × peso × modelo; agregado por painel de juízes |
| **Frequência** | Toda PR que toca prompt, modelo, RAG ou agente |

### Camada 2 — Painel multi-IA (LLM-as-judge)

Aprendi a duras penas que **um juiz** dá nota alta para si próprio. Solução: **painel**.

```
candidate_output ─┬─► Judge A (Claude Sonnet)  ─► score, justificativa
                  ├─► Judge B (GPT-4o)         ─► score, justificativa
                  ├─► Judge C (Gemini 2.5 Pro) ─► score, justificativa
                  └─► Judge D (Ollama gemma3)  ─► score, justificativa
                              │
                              ▼
                  Consenso (majority + median) ─► pass / fail / retry
```

Cada juiz responde à mesma rubrica. **Concordância < 70%** vira *flag* para humano. Eu rastreio a concordância média no dashboard de eval — quando cai, é sinal de que a rubrica está ambígua.

### Camada 3 — Verificador adversarial externo

Para tarefa de alto custo de erro (citação jurídica, alerta clínico), juiz LLM **não basta**.
Verificador externo busca a fonte real:

- **Peticionamento jurídico**: todo REsp/Tema/Súmula citado é validado em **Jusbrasil (conta logada)** + Google + STJ antes do *render*. Após detectar 3 REsps inventados em peça real (REsps 1.985.691, 1.726.292, 1.655.165 — falsos), virou regra inegociável.
- **SOAP clínico**: medicamento citado vai a base oficial (Anvisa/bula); dosagem fora de faixa dispara *alerta auto*.
- **Resumo de processo**: cada número de processo extraído vai a **DataJud (CNJ)** para confirmar existência.

### Camada 4 — Eval online (métricas reais em produção)

| Métrica | O que mede | Como coleto |
|---|---|---|
| `taxa-de-revisão-zero-emenda` | Quanto o humano aceita sem mudar | Flag em UI: "aceitar como está" |
| `tempo-mediano-por-tarefa` | Tempo do trigger ao aceite | OTel spans |
| `cost-per-resolved-task` | $$ por output aceito (não por chamada) | Soma de tokens × preço por SKU |
| `p95-latency-ms` | Latência ponta-a-ponta | OTel spans |
| `taxa-de-fail-closed` | % de tarefas que pararam por falta de verificação | Logs de guardrails |

Quando `cost-per-resolved-task > teto`, abro investigação: roteamento errado de tier? *Retry* explodindo? Modelo desnecessariamente caro?

## Anatomia de um eval suite (exemplo real — peticionamento)

```yaml
eval_suite: peticionamento_familia_v3
baseline_score: 4.2
gate: 4.0       # PR trava se < 4.0
budget:
  cost_per_resolved_task_max_usd: 0.18
  p95_latency_ms_max: 12000
dataset:
  size: 80
  source: anonimizado_pastas_internas
  split: { dev: 60, holdout: 20 }
rubric:
  - { name: tese_correta,       weight: 3, type: judge_panel }
  - { name: jurisprudencia_real, weight: 2, type: external_verifier }  # Jusbrasil
  - { name: estrutura_MV,        weight: 2, type: judge_panel }
  - { name: tom_advogado,        weight: 1, type: judge_panel }
  - { name: sem_alucinação,      weight: 5, type: external_verifier }
judges: [claude-sonnet, gpt-4o, gemini-2.5-pro, ollama-gemma3:4b]
consensus: { type: majority, min_agreement: 0.7 }
```

## Caso real — eval pegou degradação silenciosa

**NoKey S1 (RAG)**: troquei modelo de embedding por uma versão "mais nova" sem reodar eval. Score caiu 1.2 ponto em PT-BR (modelo era treinado majoritariamente em inglês). Eval pegou em CI → revertido para `multilingual-e5-large` → score voltou. Sem suíte de eval, isso teria virado bug em produção.

**Lastro / um caso interno anonimizado**: peça gerada citou 3 REsps inexistentes. Sem o **verificador externo via Jusbrasil**, teriam ido para protocolo. Esta foi a origem da regra fail-closed.

---

## 🇬🇧 English

> "An AI feature without evals is a bet." — my thesis #2.
> Anything that ships passes a 3-layer eval pipeline: **offline** (curated dataset), **online** (live metrics), **adversarial** (external verifier) — with a registered cost/latency budget.

## Principles

1. **Curated, not auto-generated.** Eval datasets are hand-built from real cases — no synthetic data on day 1.
2. **Multiple judges, not one.** A single LLM-as-judge is vulnerable to self-preference. A 3–5 model panel votes.
3. **Fail-closed.** If < majority approves, the system **stops** and returns to human. It does not ship a doubtful result.
4. **Value-tied metrics.** "ROUGE accuracy" says nothing in clinical/legal contexts. I want `zero-edit acceptance rate`, `time saved per task`, `cost-per-resolved-task`.
5. **Always regression.** Eval runs in CI on every PR. Score below baseline blocks merge.

## 4-layer pipeline

### Layer 1 — Offline eval (curated dataset)

| Item | Detail |
|---|---|
| **Size** | 50–200 cases per task type (briefs, SOAP, classification) |
| **Source** | 100% real anonymized cases — no synthetic on MVP |
| **Annotation** | Gold response + acceptance rubric per case |
| **Metric** | 0–5 score × rubric × weight × model; aggregated by judge panel |
| **Frequency** | Every PR touching prompt, model, RAG, or agent |

### Layer 2 — Multi-AI panel (LLM-as-judge)

I learned the hard way that **a single judge** gives itself high marks. Solution: **panel**.

```
candidate_output ─┬─► Judge A (Claude Sonnet)  ─► score, rationale
                  ├─► Judge B (GPT-4o)         ─► score, rationale
                  ├─► Judge C (Gemini 2.5 Pro) ─► score, rationale
                  └─► Judge D (Ollama gemma3)  ─► score, rationale
                              │
                              ▼
                  Consensus (majority + median) ─► pass / fail / retry
```

Each judge runs the same rubric. **Agreement < 70%** flags for human review. I track mean agreement on an eval dashboard — when it drops, the rubric is likely ambiguous.

### Layer 3 — External adversarial verifier

For high-cost-of-error tasks (legal citations, clinical alerts), a judge LLM **is not enough**.
An external verifier checks the real source:

- **Legal briefs**: every cited case-law identifier validated against **Jusbrasil (logged-in account)** + Google + STJ before render. After catching 3 fabricated case-law numbers in a real brief (REsps 1.985.691, 1.726.292, 1.655.165 — all fake), this became a non-negotiable rule.
- **Clinical SOAP**: every named drug goes to the official base (Anvisa/leaflet); out-of-range dosing auto-alerts.
- **Process summary**: every extracted process number checked against **DataJud (CNJ)**.

### Layer 4 — Online eval (live production metrics)

| Metric | What it measures | How I collect it |
|---|---|---|
| `zero-edit acceptance rate` | How often humans accept unchanged | UI flag: "accept as-is" |
| `median-time-per-task` | Trigger to accept | OTel spans |
| `cost-per-resolved-task` | $ per accepted output (not per call) | Tokens × SKU price |
| `p95-latency-ms` | End-to-end | OTel spans |
| `fail-closed rate` | % of tasks halted for lack of verification | Guardrails logs |

When `cost-per-resolved-task > cap`, I investigate: wrong tier routing? Retry storm? Unnecessarily expensive model?

## Anatomy of an eval suite (real example — legal briefs)

```yaml
eval_suite: family_briefs_v3
baseline_score: 4.2
gate: 4.0       # PR blocks if < 4.0
budget:
  cost_per_resolved_task_max_usd: 0.18
  p95_latency_ms_max: 12000
dataset:
  size: 80
  source: anonymized_internal_folders
  split: { dev: 60, holdout: 20 }
rubric:
  - { name: correct_thesis,        weight: 3, type: judge_panel }
  - { name: real_case_law,         weight: 2, type: external_verifier }  # Jusbrasil
  - { name: house_style_MV,        weight: 2, type: judge_panel }
  - { name: lawyer_tone,           weight: 1, type: judge_panel }
  - { name: no_hallucination,      weight: 5, type: external_verifier }
judges: [claude-sonnet, gpt-4o, gemini-2.5-pro, ollama-gemma3:4b]
consensus: { type: majority, min_agreement: 0.7 }
```

## Real case — eval caught silent degradation

**NoKey S1 (RAG)**: swapped embedding model for a "newer" one without re-running eval. PT-BR score dropped 1.2 points (the model was English-heavy). CI caught it → reverted to `multilingual-e5-large` → score recovered. Without an eval suite, this would have been a production bug.

**Lastro / an anonymized internal case**: a generated brief cited 3 nonexistent REsps. Without the **external verifier through Jusbrasil**, they would have gone to filing. This was the origin of the fail-closed rule.
