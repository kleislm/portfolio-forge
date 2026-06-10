# Evidence map — Innovation Manager × AI Engineer

🇧🇷 Português (principal) · [🇬🇧 English ↓](#-english)

> **Sem nota autoatribuída.** Dizer "sou 4 em RAG" não prova nada — apontar para o produto onde o RAG roda, prova. Cada competência abaixo linka para onde está demonstrada. Quando a evidência é fraca ou em construção, está dito.

## Eixo 1 — Gestão de Inovação & Produto

### Execução de Produto

| Competência | Onde está provada |
|---|---|
| Especificação & arquitetura de feature | 14 produtos com case study no [índice](../case-studies/_index.md); specs ponta a ponta em [Lastro](../case-studies/lastro.md) e [Assist4Doc](../case-studies/assist4doc.md) |
| Entrega ponta a ponta | 6 produtos no ar com domínio próprio: [assist4doc.com](https://assist4doc.com) · [pamellamonteiro.com](https://pamellamonteiro.com) · [pmforge.com.br](https://pmforge.com.br) · [klmadvogados.com](https://klmadvogados.com) + 2 em lovable.app |
| Qualidade & disciplina de teste | [NoKey](https://github.com/kleislm/nokey): ~39 testes automatizados, CI com badge, benchmark honesto por módulo |

### Insight de Cliente

| Competência | Onde está provada |
|---|---|
| Voz do cliente | Discovery com médicos ([Assist4Doc](../case-studies/assist4doc.md)), advogados ([Lastro](../case-studies/lastro.md)), criadores ([Pamella's Hub](../case-studies/pamella-ai-hub.md)) — três personas distintas, três produtos no ar |
| UX / craft | Design systems próprios, multi-idioma e offline-first nos cases; superfícies de conversão em [Leone](../case-studies/leone-growth.md) e [Desburocratize](../case-studies/desburocratize.md) |
| Resultado de negócio | 3 empresas fundadas e operadas com responsabilidade por caixa e aquisição — [história de founder](../ventures/README.md) |

### Estratégia & Inovação

| Competência | Onde está provada |
|---|---|
| Portfólio H1/H2/H3 | Tese formal aplicada e documentada no [Playbook de Inovação](../innovation/playbook.md), com cases mapeados por horizonte |
| Governança de IA & IA Responsável | 5 controles operacionais no [playbook §3](../innovation/playbook.md); human-in-the-loop como arquitetura em [Assist4Doc](../case-studies/assist4doc.md) e [Lastro](../case-studies/lastro.md) |
| Visão & roadmap | Posicionamento das bandeiras em domínio regulado — [bio & tese](bio.md) |
| Impacto estratégico | **Em construção declarada**: conectar bandeiras a North Star metric em escala de organização (não só venture própria) |

### Influência

| Competência | Onde está provada |
|---|---|
| Stakeholder management | Transferível de negociação jurídica/contratual; evidência em escala de org **ainda a construir** |
| Liderança de time | Operação liderada nas próprias empresas — [ventures](../ventures/README.md) |
| Change management | Adoção de IA em time jurídico próprio (fluxo Ato→Peça do [Lastro](../case-studies/lastro.md)); sequência de discovery no [playbook §4](../innovation/playbook.md) |

## Eixo 2 — AI Engineering

| Competência | Onde está provada |
|---|---|
| **LLM apps & integração** | OpenAI, Anthropic, Gemini e Ollama local em produção — [ai-stack.md](../engineering/ai-stack.md) |
| **RAG** | Híbrido BM25+denso (RRF) + MMR em código aberto auditável: [`@nokey/rag`](https://github.com/kleislm/nokey) (13/13 testes); verificador adversarial no [Lastro](../case-studies/lastro.md) |
| **Agentes (LangGraph)** | *Verifier loop* + 59 agentes especializados — [architecture-patterns.md §2](../engineering/architecture-patterns.md) |
| **Evals (offline + online)** | Pipeline de 4 camadas com painel multi-IA e caso real de regressão detectada — [eval-methodology.md](../engineering/eval-methodology.md) |
| **Guardrails & fail-closed** | Padrão documentado com código — [architecture-patterns.md §4](../engineering/architecture-patterns.md); origem: alucinação real de citações detectada por verificador externo |
| **Modelos locais** | Ollama (gemma3, qwen3, llama3), WebLLM, faster-whisper — demos no navegador em [NoKey](https://github.com/kleislm/nokey) (DevTools → zero API calls) |
| **Backend Python** | FastAPI + APScheduler/arq + PostgreSQL + WebSockets em produção — [ai-stack.md](../engineering/ai-stack.md) |
| **TypeScript/React** | React 19 + Vite + Tailwind + shadcn/ui nos produtos no ar; módulos TS no [NoKey](https://github.com/kleislm/nokey) |
| **Autz fine-grained** | OpenFGA + OPA multi-tenant — [architecture-patterns.md §6](../engineering/architecture-patterns.md) |
| **Seleção & roteamento de modelo** | Painel multi-IA + roteamento por tier custo×qualidade — [ai-stack.md](../engineering/ai-stack.md) e [eval-methodology.md](../engineering/eval-methodology.md) |
| **MLOps** | **Nível básico, dito com franqueza**: prompt versionado, eval em CI, monitoring de custo/qualidade. Sem feature store nem treino de modelo próprio — e não finjo que tenho |

## Leitura honesta

- **Mais forte (evidência pública e auditável):** execução ponta a ponta, AI engineering aplicado (RAG, evals, guardrails), domínio regulado, growth de founder.
- **Em construção (dito sem maquiagem):** impacto estratégico e influência em escala de organização — o pulo de "faço acontecer sozinho" para "faço a organização acontecer". É exatamente o que busco no próximo papel.
- **Diferencial não óbvio:** gestor de inovação **que constrói o sistema de IA**, com base jurídica real. A evidência está nos links — não na nota.

---

## 🇬🇧 English

> **No self-assigned scores.** Saying "I'm a 4 in RAG" proves nothing — pointing to the product where the RAG runs, does. Every competency below links to where it's demonstrated. Where the evidence is weak or under construction, it says so.

## Axis 1 — Innovation & Product Management

### Product Execution

| Competency | Where it's proven |
|---|---|
| Feature spec & architecture | 14 products with case studies in the [index](../case-studies/_index.md); end-to-end specs in [Lastro](../case-studies/lastro.md) and [Assist4Doc](../case-studies/assist4doc.md) |
| End-to-end delivery | 6 products live on their own domains: [assist4doc.com](https://assist4doc.com) · [pamellamonteiro.com](https://pamellamonteiro.com) · [pmforge.com.br](https://pmforge.com.br) · [klmadvogados.com](https://klmadvogados.com) + 2 on lovable.app |
| Quality & test discipline | [NoKey](https://github.com/kleislm/nokey): ~39 automated tests, CI badge, honest per-module benchmark |

### Customer Insight

| Competency | Where it's proven |
|---|---|
| Voice of the customer | Discovery with physicians ([Assist4Doc](../case-studies/assist4doc.md)), lawyers ([Lastro](../case-studies/lastro.md)), creators ([Pamella's Hub](../case-studies/pamella-ai-hub.md)) — three distinct personas, three live products |
| UX / craft | Own design systems, multilingual and offline-first across cases; conversion surfaces in [Leone](../case-studies/leone-growth.md) and [Desburocratize](../case-studies/desburocratize.md) |
| Business outcome | 3 companies founded and operated with cash and acquisition ownership — [founder story](../ventures/README.md) |

### Strategy & Innovation

| Competency | Where it's proven |
|---|---|
| H1/H2/H3 portfolio | Formal thesis applied and documented in the [Innovation Playbook](../innovation/playbook.md), cases mapped per horizon |
| AI Governance & Responsible AI | 5 operational controls in [playbook §3](../innovation/playbook.md); human-in-the-loop as architecture in [Assist4Doc](../case-studies/assist4doc.md) and [Lastro](../case-studies/lastro.md) |
| Vision & roadmap | Flagship positioning in regulated domains — [bio & thesis](bio.md) |
| Strategic impact | **Openly under construction**: tying flagships to a North Star metric at org scale (beyond my own ventures) |

### Influence

| Competency | Where it's proven |
|---|---|
| Stakeholder management | Transferable from legal/contract negotiation; org-scale evidence **still to build** |
| Team leadership | Led operations in my own companies — [ventures](../ventures/README.md) |
| Change management | AI adoption in my own legal team (Lastro's Act→Brief flow); discovery sequence in [playbook §4](../innovation/playbook.md) |

## Axis 2 — AI Engineering

| Competency | Where it's proven |
|---|---|
| **LLM apps & integration** | OpenAI, Anthropic, Gemini, and local Ollama in production — [ai-stack.md](../engineering/ai-stack.md) |
| **RAG** | Hybrid BM25+dense (RRF) + MMR in auditable open source: [`@nokey/rag`](https://github.com/kleislm/nokey) (13/13 tests); adversarial verifier in [Lastro](../case-studies/lastro.md) |
| **Agents (LangGraph)** | Verifier loop + 59 specialized agents — [architecture-patterns.md §2](../engineering/architecture-patterns.md) |
| **Evals (offline + online)** | 4-layer pipeline with multi-AI panel and a real caught-regression case — [eval-methodology.md](../engineering/eval-methodology.md) |
| **Guardrails & fail-closed** | Pattern documented with code — [architecture-patterns.md §4](../engineering/architecture-patterns.md); origin: real citation hallucination caught by an external verifier |
| **Local models** | Ollama (gemma3, qwen3, llama3), WebLLM, faster-whisper — in-browser demos in [NoKey](https://github.com/kleislm/nokey) (DevTools → zero API calls) |
| **Python backend** | FastAPI + APScheduler/arq + PostgreSQL + WebSockets in production — [ai-stack.md](../engineering/ai-stack.md) |
| **TypeScript/React** | React 19 + Vite + Tailwind + shadcn/ui across live products; TS modules in [NoKey](https://github.com/kleislm/nokey) |
| **Fine-grained authz** | OpenFGA + OPA multi-tenant — [architecture-patterns.md §6](../engineering/architecture-patterns.md) |
| **Model selection & routing** | Multi-AI panel + cost×quality tier routing — [ai-stack.md](../engineering/ai-stack.md) and [eval-methodology.md](../engineering/eval-methodology.md) |
| **MLOps** | **Basic level, said plainly**: versioned prompts, evals in CI, cost/quality monitoring. No feature store, no own model training — and I don't pretend otherwise |

## Honest read

- **Strongest (public, auditable evidence):** end-to-end execution, applied AI engineering (RAG, evals, guardrails), regulated domains, founder-grade growth.
- **Under construction (no makeup):** strategic impact and influence at org scale — the jump from "I make it happen alone" to "I make the organization happen". Exactly what I'm looking for in the next role.
- **Non-obvious edge:** an innovation manager **who builds the AI system**, with real legal grounding. The evidence is in the links — not in a score.
