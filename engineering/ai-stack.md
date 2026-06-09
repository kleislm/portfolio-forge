# AI Stack — o que opero em produção

🇧🇷 Português · [🇬🇧 English ↓](#-english)

Stack consolidado nos meus produtos atuais (Lastro, Assist4Doc, NoKey). Não é "stack favorito teórico" — é o que efetivamente rodou e tem prova nos cases.

## Modelos

| Camada | Escolha | Quando uso |
|---|---|---|
| Espinha (Opus-tier) | Anthropic Claude Opus | Decisão crítica, agente orquestrador, *parse* jurídico complexo |
| Grosso (Sonnet-tier) | Claude Sonnet · GPT-4o · Gemini 2.5 Pro | Volume principal — geração, sumarização, análise |
| Trivial (Haiku-tier) | Claude Haiku · Gemini Flash · gpt-4o-mini | Roteamento, classificação, *extract* de campo |
| Local | Ollama (`gemma3:4b`, `qwen3:4b`, `llama3`) · WebLLM | Dev/eval offline, dados sensíveis, custo zero por token |
| ASR | faster-whisper (Whisper.cpp) · Web Speech API | Transcrição clínica/jurídica local |

**Roteamento por custo×qualidade**: cada chamada carrega `tarefa.tier`. Se `tier == espinha` → Opus; senão `tier == grosso` → Sonnet; senão Haiku. Painel multi-IA decide qual modelo "vence" por tarefa via eval automático.

## Orquestração

- **LangGraph** para agentes com *verifier loop* — o agente nunca entrega sem o verificador adversarial confirmar.
- **LangChain** para grafos simples (chains lineares, retrievers).
- **Agentes especializados por domínio** — 59 agentes jurídicos no Lastro (família, cobrança, recurso, HC, MS etc.), cada um com escopo e prompt versionado.
- **Tool-use estruturado** — JSON Schema obrigatório; falha → retry com correção de schema.

## RAG

- **Embeddings**: `multilingual-e5-large` (multilingual-e5 corrigiu fraqueza em PT pego em eval da NoKey S1) ou `text-embedding-3-large` quando cliente paga.
- **Híbrido BM25 + denso** com fusão **RRF** (Reciprocal Rank Fusion).
- **MMR** (Maximal Marginal Relevance) para diversidade — evita 5 chunks dizendo a mesma coisa.
- **Verificador adversarial obrigatório** para citação jurídica — após detectar **REsps inventados** em peça real (um caso interno anonimizado), todo precedente citado passa por validação cruzada Jusbrasil/STJ/Google antes do *render*.
- **Banco de teses**: indexação separada das teses do operador (por tenant), com escopo (Civil, Família, Trabalho), data de validade e *grounding* obrigatório.

## Backend

- **FastAPI** (Python 3.12+) — endpoints async, OpenAPI tipado, contrato versionado.
- **APScheduler / arq** — *timer durável* (substitui `sleep` em loop). Tarefas longas saem do request.
- **PostgreSQL** + extensões `pgvector` para *embeddings* quando faz sentido manter no banco.
- **WebSockets** para *streaming* de resposta (token-a-token) e *progress* de agente longo.
- **OpenFGA + OPA** — autorização *fine-grained* multi-tenant (cliente vs. caso vs. pasta).
- **Zitadel** — Identity Provider (escolhi sobre Keycloak por API moderna e tenancy nativa).

## Frontend

- **React 19** · **Vite** · **TypeScript** · **Tailwind** · **shadcn/ui**.
- **SSR seletivo** (Next.js) — só onde converte. Decisão de Assist4Doc Site: SSR só na landing, app fica SPA.
- **Streaming UI** — *typewriter* nativo via WebSocket, sem bloquear o thread.

## Evals & Guardrails

- **LLM-as-judge multi-painel** — 3 a 5 juízes independentes votam; *fail-closed* se < maioria aprovar.
- **Dataset curado** por tipo de tarefa (peticionamento, SOAP, classificação de email etc.).
- **Regression suite** em CI — *score* mínimo por *commit*; PR não passa sem.
- **Custo & latência budget** — cada feature tem `cost_per_resolved_task_max` e `p95_latency_ms_max`.
- **Fail-closed** em decisão crítica — se o sistema não consegue verificar (ex.: REsp não encontrado em base), ele para e devolve "**preciso de revisão humana**".

## Infraestrutura

- **Docker** — *compose* para dev, multi-stage build pra prod.
- **Supabase Edge Functions** quando faz sentido (rápido para MVP, baixo *ops*).
- **Deploy blue-green** + **restore test mensal** — banco *snapshot* restaurado em ambiente isolado.
- **Monitoring + tracing** — *spans* OpenTelemetry de ponta a ponta (request → embeddings → LLM → verifier → response), métricas de custo/latência/qualidade.

## Política de custo × qualidade (Max 20x)

Aprendi a **nunca economizar no *fail-closed***. O modelo barato pode escrever a peça; o verificador caro (Opus + busca real no Jusbrasil) precisa confirmar. ROI da espinha cara é a alucinação que ela impede.

→ Detalhe completo em [architecture-patterns.md](architecture-patterns.md) e [eval-methodology.md](eval-methodology.md).

---

## 🇬🇧 English

Consolidated stack across my current products (Lastro, Assist4Doc, NoKey). Not a "theoretical favorite stack" — what actually shipped and has proof in the cases.

## Models

| Tier | Choice | When I use it |
|---|---|---|
| Backbone (Opus-tier) | Anthropic Claude Opus | Critical decisions, orchestrator agent, complex legal parsing |
| Bulk (Sonnet-tier) | Claude Sonnet · GPT-4o · Gemini 2.5 Pro | Main volume — generation, summarization, analysis |
| Trivial (Haiku-tier) | Claude Haiku · Gemini Flash · gpt-4o-mini | Routing, classification, field extraction |
| Local | Ollama (`gemma3:4b`, `qwen3:4b`, `llama3`) · WebLLM | Dev/eval offline, sensitive data, zero token cost |
| ASR | faster-whisper · Web Speech API | Local clinical/legal transcription |

**Cost×quality routing**: each call carries `task.tier`. If `tier == backbone` → Opus; else `tier == bulk` → Sonnet; else Haiku. A multi-AI panel picks which model "wins" per task via automated eval.

## Orchestration

- **LangGraph** for verifier-loop agents — agent never ships without an adversarial verifier confirming.
- **LangChain** for simple graphs (linear chains, retrievers).
- **Domain-specialized agents** — 59 legal agents in Lastro (family, collection, appeal, HC, MS etc.), each with versioned scope and prompt.
- **Structured tool-use** — mandatory JSON Schema; failure → retry with schema correction.

## RAG

- **Embeddings**: `multilingual-e5-large` (multilingual-e5 fixed a PT weakness caught by NoKey S1 eval) or `text-embedding-3-large` when the client pays.
- **Hybrid BM25 + dense** with **RRF** fusion.
- **MMR** for diversity — avoids 5 chunks saying the same thing.
- **Mandatory adversarial verifier** for legal citations — after catching **fabricated case-law numbers** in a real brief (an anonymized internal case), every precedent goes through cross-validation Jusbrasil/STJ/Google before render.
- **Thesis bank**: separate indexing of the firm's positions, with scope (Civil, Family, Labor), expiry, and mandatory grounding.

## Backend

- **FastAPI** (Python 3.12+) — async endpoints, typed OpenAPI, versioned contract.
- **APScheduler / arq** — durable timer (replaces `sleep`-in-a-loop). Long tasks leave the request.
- **PostgreSQL** + `pgvector` for embeddings when it makes sense to keep them in DB.
- **WebSockets** for token streaming and long-agent progress.
- **OpenFGA + OPA** — fine-grained multi-tenant authorization (client vs. case vs. folder).
- **Zitadel** — identity provider (chose it over Keycloak for modern API and native tenancy).

## Frontend

- **React 19** · **Vite** · **TypeScript** · **Tailwind** · **shadcn/ui**.
- **Selective SSR** (Next.js) — only where it converts. Assist4Doc Site decision: SSR for landing, app stays SPA.
- **Streaming UI** — native typewriter over WebSocket, non-blocking.

## Evals & Guardrails

- **Multi-panel LLM-as-judge** — 3–5 independent judges vote; fail-closed if < majority approves.
- **Curated dataset** per task type (briefs, SOAP, email classification etc.).
- **Regression suite** in CI — minimum score per commit; PR blocks if not met.
- **Cost & latency budget** — every feature has `cost_per_resolved_task_max` and `p95_latency_ms_max`.
- **Fail-closed** on critical calls — if the system can't verify (e.g., case-law number not found in source), it stops and returns "**needs human review**".

## Infrastructure

- **Docker** — compose for dev, multi-stage build for prod.
- **Supabase Edge Functions** when it makes sense (fast MVP, low ops).
- **Blue-green deploy** + **monthly restore test** — DB snapshot restored to isolated env.
- **Monitoring + tracing** — end-to-end OpenTelemetry spans (request → embeddings → LLM → verifier → response), with cost/latency/quality metrics.

## Cost × quality policy (Max 20x)

The lesson: **never economize on fail-closed.** The cheap model can draft the brief; the expensive verifier (Opus + real Jusbrasil search) must confirm. The ROI of the expensive backbone is the hallucination it prevents.

→ Full detail in [architecture-patterns.md](architecture-patterns.md) and [eval-methodology.md](eval-methodology.md).
