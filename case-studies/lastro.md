# Lastro — plataforma jurídica AI-native ⭐

<!-- TODO: criar assets/lastro.png próprio (atualmente sem hero image) -->

🇧🇷 Português · [🇬🇧 English ↓](#-english)

**Papel:** Founder · Innovation Manager · AI Engineer · Builder &nbsp;|&nbsp; **Status:** Em construção (Estágio A — 9 sprints S0–S8 documentadas) &nbsp;|&nbsp; **Domínio:** Legaltech (regulado)
🔗 Spinout OSS: [nokey](https://github.com/kleislm/nokey) · Repositórios principais privados (stealth)

> **Lastro ≠ KLM Legal Hub.** São **dois produtos distintos**. Lastro é uma **plataforma** jurídica AI-native (multi-tenant, vendável a qualquer banca). KLM Legal Hub é o produto **próprio do escritório KLM Advogados** ([klmadvogados.com](https://klmadvogados.com)) — pré-IA, foco em Kanban + landing — ver [caso próprio](klm-legal-hub.md).

### Contexto

Banca jurídica trabalha em três operações desconectadas: **(1)** receber publicação/intimação, **(2)** decidir tese e peça, **(3)** produzir a peça e protocolar. Cada bloco tem ferramentas próprias (DJEN, Jusbrasil, Word, PJe), sem fio condutor de dados. O resultado: retrabalho, peças com qualidade desigual entre advogados, alucinação quando alguém tenta "ChatGPT genérico", e ZERO governança sobre o que a IA gera.

### Problema

Construir uma plataforma jurídica **AI-native** onde:
1. **A IA propõe, o advogado decide** — humano-no-loop em toda peça crítica.
2. **Citação alucinada é impossível** — todo precedente passa por verificador adversarial real (Jusbrasil, STJ, ConJur, Google) **antes** de o draft chegar ao usuário.
3. **Multi-modelo** — o melhor modelo para cada tarefa, escolhido por painel de eval, não por preferência.
4. **Multi-sócio, multi-cliente, escopo fino** — sócio A não vê pasta do sócio C.
5. **Cabe em qualquer banca** — Mac, Linux, Windows; deploy próprio possível.

### Innovation lens (H1 + H2 + H3 simultâneos)

| Horizonte | Aposta | Estado |
|---|---|---|
| **H1** | Geração assistiva de peças (DNA do operador em template padronizado) | Em produção interna em banca piloto |
| **H2** | Novo *operating model* — fluxo Ato→Peça (intimação automaticamente abre o agente certo) com 59 agentes especializados; muda o trabalho do advogado de redator para revisor de tese | Em rampa |
| **H3** | Spinout OSS via **NoKey** — infraestrutura de RAG, transcribe, browser-agent, DocAI cortada do core como toolkit. Opcionalidade comercial e *flywheel* de marca | NoKey lançado, 6 módulos, ~39 testes |

### AI Engineering decisions

| Decisão | O que escolhi | Por quê |
|---|---|---|
| **Painel multi-IA para escolher modelo** | 4 modelos (Claude, GPT-4o, Gemini, Ollama) competem por tarefa; vencedor por eval | Não dá pra confiar em benchmark genérico em domínio jurídico-BR |
| **Verified RAG obrigatório** | Toda citação valida em Jusbrasil/STJ/Google antes do render | Detectei 3 REsps inventados (um caso interno anonimizado) — virou regra inegociável |
| **Fail-closed em citação não verificada** | Sistema para e devolve "preciso de revisão humana" em vez de entregar peça duvidosa | Em jurídico, "errado com confiança" gera passivo profissional grave |
| **Verifier loop em LangGraph** | Agente especializado por matéria (59 agentes) + verificador adversarial pré-entrega | Diversidade de erro: verificador é modelo diferente do gerador |
| **OpenFGA + OPA** para autz | Relacional, estilo Zanzibar | RBAC tradicional não modela "sócio A vê cliente B mas só pasta C" |
| **Zitadel** como IdP | Sobre Keycloak | API moderna, tenancy nativa |
| **APScheduler / arq** para tarefa longa | Em vez de `sleep`-in-loop ou subprocess | Timer durável, sobrevive a restart, observa progresso |
| **Banco de teses por operador** | Indexação separada do RAG geral, por tenant | Tese própria tem escopo e validade — não dá pra misturar com base livre |
| **Ollama local como drafter rápido** | `gemma3:4b` | Dados sensíveis em desenvolvimento ficam no laptop; custo zero em iteração |
| **Roteamento Opus/Sonnet/Haiku por tier** | Tabela de tier por tarefa, fail-closed sempre tier `backbone` | Unit economics + qualidade onde importa |
| **Deploy blue-green + restore test mensal** | Snapshot do Postgres restaurado em ambiente isolado todo mês | "Tenho backup" sem teste é fé; com teste é fato |

### Stack consolidado

`FastAPI` · `PostgreSQL + pgvector` · `WebSockets` · `LangGraph` · `OpenFGA + OPA` · `Zitadel` · `APScheduler/arq` · `React 19 + Vite + Tailwind + shadcn/ui` · `Ollama (gemma3, qwen3, llama3)` · `Anthropic Claude + OpenAI + Gemini`.

Detalhe técnico: [`engineering/ai-stack.md`](../engineering/ai-stack.md), [`eval-methodology.md`](../engineering/eval-methodology.md), [`architecture-patterns.md`](../engineering/architecture-patterns.md).

### Decisões que recusei

- **"Construir nosso próprio LLM"** — irrelevante. O moat é o domínio jurídico + verificador + dataset interno.
- **"Tudo no GPT-4o"** — quebra unit economics em volume e dá ao fornecedor uma alavanca de preço.
- **"Eval depois"** — não. Eval no MVP, eval no CI, ou não vai pra produção.
- **"Confiamos no LLM"** — confiamos no verificador. Sem verifier loop, é roleta-russa de passivo.

### Impacto (estado atual)

- **9 sprints S0–S8 documentadas** + Pós-A (vault Obsidian interno).
- **Frontend vivo** (Sprint 7): React 19 + Vite ligado ao FastAPI; loop UI → IA → verificador funcionando.
- **Drafter rápido**: Ollama `gemma3:4b` provado em dev (qwen3:4b ignora `think:false` no Ollama 0.30.7 — bug pego em eval).
- **NoKey (spinout OSS)**: catálogo de 6 módulos completo, ~39 testes, benchmark honesto vs. unicórnio que cada um substitui.
- **Biblioteca de templates** consolidada a partir de 17 peças complexas reais (todas anonimizadas), incluindo uma peça-bandeira para carteira de fraude que virou template replicável.
- **Política de custo×qualidade Max 20x** documentada e em vigor — vira CLAUDE.md do repo.

### Por que este caso me define

Combina os três eixos que tento juntar: **gestão de inovação** (H1+H2+H3 simultâneos, governança, change), **AI engineering** (LangGraph, verified RAG, evals, multi-IA panel, autz fino) e **domínio regulado** (jurídico BR, LGPD, ética profissional, fail-closed). Eu não falo sobre — eu construí.

> Detalhes internos e métodos proprietários foram omitidos. A banca piloto é cliente real e os dados são sensíveis.

---

## 🇬🇧 English

**Role:** Founder · Innovation Manager · AI Engineer · Builder &nbsp;|&nbsp; **Status:** Building (Stage A — 9 sprints S0–S8 documented) &nbsp;|&nbsp; **Domain:** Legaltech (regulated)
🔗 OSS spinout: [nokey](https://github.com/kleislm/nokey) · Main repos private (stealth)

> **Lastro ≠ KLM Legal Hub.** They are **two distinct products**. Lastro is an AI-native legal **platform** (multi-tenant, sellable to any firm). KLM Legal Hub is the law firm KLM Advogados' **own product** ([klmadvogados.com](https://klmadvogados.com)) — pre-AI, Kanban + landing focus — see [its case](klm-legal-hub.md).

### Context

A law firm works in three disconnected operations: **(1)** receive notices/communications, **(2)** decide thesis and brief type, **(3)** produce the brief and file. Each block has its own tools (court electronic systems, case-law databases, Word, filing portals), with no data continuity. Result: rework, uneven quality across lawyers, hallucinations when someone uses "generic ChatGPT", and **zero governance** over what AI generates.

### Problem

Build an **AI-native** legal platform where:
1. **AI proposes, lawyer decides** — human-in-the-loop on every critical brief.
2. **Hallucinated citations are impossible** — every precedent goes through an adversarial verifier (Jusbrasil, STJ, ConJur, Google) **before** the draft reaches the user.
3. **Multi-model** — best model per task, picked by eval panel, not by preference.
4. **Multi-partner, multi-client, fine-grained scope** — partner A doesn't see partner C's folder.
5. **Runs anywhere** — Mac, Linux, Windows; self-host possible.

### Innovation lens (simultaneous H1 + H2 + H3)

| Horizon | Bet | Status |
|---|---|---|
| **H1** | Assistive brief generation (operator's DNA in standardized template) | In internal production at pilot firm |
| **H2** | New operating model — Act→Brief flow (a notification automatically opens the right agent) with 59 specialized agents; shifts lawyer's work from drafter to thesis reviewer | In ramp |
| **H3** | OSS spinout via **NoKey** — RAG, transcribe, browser-agent, DocAI infra cut from the core as a toolkit. Commercial optionality and brand flywheel | NoKey shipped, 6 modules, ~39 tests |

### AI Engineering decisions

| Decision | What I chose | Why |
|---|---|---|
| **Multi-AI panel to pick the model** | 4 models (Claude, GPT-4o, Gemini, Ollama) compete per task; winner by eval | Can't trust generic benchmarks in PT-BR legal domain |
| **Mandatory verified RAG** | Every citation validated against Jusbrasil/STJ/Google before render | Caught 3 fabricated case-law numbers (an anonymized internal case) — became a non-negotiable rule |
| **Fail-closed on unverified citation** | System halts and returns "needs human review" instead of shipping a doubtful brief | In legal, "wrong with confidence" creates serious professional liability |
| **Verifier loop in LangGraph** | Domain-specialized agents (59) + adversarial verifier before delivery | Error diversity: verifier is a different model from the generator |
| **OpenFGA + OPA** for authz | Relational, Zanzibar-style | RBAC doesn't model "partner A sees client B but only folder C" |
| **Zitadel** as IdP | Over Keycloak | Modern API, native tenancy |
| **APScheduler / arq** for long tasks | Instead of sleep-loops or subprocesses | Durable timer, survives restart, observes progress |
| **Firm thesis bank** | Separately indexed from generic RAG | Firm's positions have scope and expiry — can't mix with open base |
| **Local Ollama as fast drafter** | `gemma3:4b` | Sensitive dev data stays on the laptop; zero iteration cost |
| **Opus/Sonnet/Haiku routing by tier** | Per-task tier table, fail-closed always `backbone` tier | Unit economics + quality where it matters |
| **Blue-green deploy + monthly restore test** | Postgres snapshot restored to isolated env every month | "I have backups" untested is faith; tested is fact |

### Consolidated stack

`FastAPI` · `PostgreSQL + pgvector` · `WebSockets` · `LangGraph` · `OpenFGA + OPA` · `Zitadel` · `APScheduler/arq` · `React 19 + Vite + Tailwind + shadcn/ui` · `Ollama (gemma3, qwen3, llama3)` · `Anthropic Claude + OpenAI + Gemini`.

Technical detail: [`engineering/ai-stack.md`](../engineering/ai-stack.md), [`eval-methodology.md`](../engineering/eval-methodology.md), [`architecture-patterns.md`](../engineering/architecture-patterns.md).

### Decisions I declined

- **"Build our own LLM"** — irrelevant. The moat is the legal domain + verifier + internal dataset.
- **"All on GPT-4o"** — breaks unit economics at scale and gives the vendor pricing leverage.
- **"Eval later"** — no. Eval in MVP, eval in CI, or no production.
- **"We trust the LLM"** — we trust the verifier. Without a verifier loop, it's liability roulette.

### Impact (current state)

- **9 sprints S0–S8 documented** + Post-A (internal Obsidian vault).
- **Frontend live** (Sprint 7): React 19 + Vite wired to FastAPI; UI → AI → verifier loop working.
- **Fast drafter**: Ollama `gemma3:4b` proven in dev (qwen3:4b ignores `think:false` on Ollama 0.30.7 — bug caught by eval).
- **NoKey (OSS spinout)**: 6-module catalog complete, ~39 tests, honest benchmark vs. the unicorn each replaces.
- **Template library** consolidated from 17 real complex briefs (all anonymized), including a flagship brief for a fraud-portfolio that became a replicable template.
- **Max 20x cost×quality policy** documented and in force — repo's `CLAUDE.md`.

### Why this case defines me

Combines the three axes I try to join: **innovation management** (simultaneous H1+H2+H3, governance, change), **AI engineering** (LangGraph, verified RAG, evals, multi-AI panel, fine-grained authz) and **regulated domain** (PT-BR legal, LGPD, professional ethics, fail-closed). I don't talk about it — I built it.

> Internal details and proprietary methods are intentionally omitted. The pilot firm is a real client and the data is sensitive.
