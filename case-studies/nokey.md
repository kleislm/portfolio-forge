# NoKey — drop-in, no-API replacements for paid APIs

🇬🇧 English · [🇧🇷 Português](#-português)

**Role:** Creator · PM · Builder &nbsp;|&nbsp; **Status:** Shipping (OSS, in sprints) &nbsp;|&nbsp; **Repo:** github.com/kleislm/nokey

### The insight
Half of what developers pay APIs for is just *compute* — and compute now runs in the browser (WASM/WebGPU) or in a free self-hosted function. NoKey is an honest, drop-in toolkit that replaces the **expensive, non-trivial** paid APIs/MCPs (not the trivia anyone codes in five lines).

### Catalog complete — 6 modules, ~39 automated tests, each with an honest benchmark vs the unicorn it replaces
- **`@nokey/llm-local` (S6)** — run LLMs with no paid tokens: OpenAI-compatible client for local runtimes (Ollama/LM Studio/vLLM) + live WebLLM browser chat. 4/4 tests.
- **`@nokey/transcribe` (S5)** — self-hosted Whisper API: local STT (faster-whisper) + SRT/VTT/Markdown + **diarization overlay** ("who spoke"). Live browser demo (Web Speech, no key). 6/6 tests.
- **`@nokey/browser-agent` (S4)** — self-hosted Browserbase: local Playwright automation (flow DSL: goto/click/type/extract/screenshot) + optional LLM agent. $0, cookies stay local. 4/4 tests incl. a real Chromium run.
- **`@nokey/docai` (S3)** — self-hosted LlamaParse/Textract: PDF → Markdown + tables + field extraction + optional OCR. Python, $0/page, docs stay local. 3/3 tests. Live in-browser demo (pdf.js).
- **`@nokey/web-markdown` (S2)** — self-hosted Firecrawl: scrape (multi-format) · crawl · map · batch · extract (CSS selectors) · change-tracking · optional JS render. Python edge, $0/page, your infra. 9/9 tests. Honest matrix vs Firecrawl in `BENCHMARK.md` (own the 80%, transparent on the anti-bot 20%).
- **`@nokey/rag` (S1, upgraded)** — production retrieval: metadata filtering + **hybrid BM25+dense (RRF)** + MMR diversity, 100% local. 13/13 tests. Beats a naive OpenAI-Embeddings + Pinecone setup on quality, at $0.

### `@nokey/rag` (S1)
Local semantic search / RAG that kills **two** bills at once — OpenAI Embeddings + Pinecone — running on the user's device.
- Real multilingual embeddings via transformers.js + a cosine vector store, persisted as JSON. Zero API key.
- **Validated, not claimed:** a Portuguese query with almost no shared words ranks the correct passage first (score 0.84); 7/7 automated tests.
- Honest by design: ships a labeled non-semantic fallback for offline tests and refuses to pretend it's the retriever.

### How I build it (the discipline)
Every module follows the same gate: build → validate → test → polish → fix → re-validate → bilingual docs → publish. S1 caught a real quality gap (an English model was mushy in Portuguese), which I fixed by switching to multilingual-e5 — exactly what the pipeline is for.

### Innovation lens
**Horizon 3 — strategic option.** A spinout of the Lastro infra layer that becomes a public OSS toolkit. Three returns at once: (1) cuts infra CAC for my own products by 80–100%; (2) builds brand and developer trust; (3) creates optionality for future commercial offers (managed-hosting, enterprise support).

### AI Engineering decisions
| Decision | What I chose | Why |
|---|---|---|
| **Honest benchmark, not marketing** | Every module ships `BENCHMARK.md` vs the unicorn it replaces; owns the 80% it wins, is transparent about the 20% it doesn't | Trust is the product; lying to the user is the fastest way to kill an OSS project |
| **Multilingual-first** | `multilingual-e5-large` over English-tuned models for embeddings | PT/ES users were getting noisy retrieval — eval caught it, I fixed |
| **Hybrid BM25 + dense + RRF + MMR for `@nokey/rag`** | Hybrid retrieval beats naive embeddings on quality and resilience to keyword queries | At $0 cost vs. Pinecone, the win is double |
| **In-browser demos** (pdf.js, WebLLM, Web Speech) | Lower barrier — user tries before installing | OSS adoption is friction-bound |
| **Tested, not slideware** | ~39 automated tests across 6 modules; PR gate | "It works on my laptop" doesn't survive contact with users |

### Why it matters
Shows I ship **real, tested infrastructure** (not slideware), make honest engineering trade-offs, and understand the cost/lock-in pain that every builder feels.

---

## 🇧🇷 Português

**Papel:** Criador · PM · Builder &nbsp;|&nbsp; **Status:** Em entrega (OSS, em sprints) &nbsp;|&nbsp; **Repo:** github.com/kleislm/nokey

### A sacada
Metade do que se paga em API é só *compute* — e compute hoje roda no navegador (WASM/WebGPU) ou numa função grátis self-host. NoKey é um kit honesto e drop-in que substitui as APIs/MCPs **caras e não-triviais** (não a trivialidade que qualquer um faz em cinco linhas).

### Entregue: `@nokey/rag` (S1)
Busca semântica / RAG local que mata **duas** contas de uma vez — OpenAI Embeddings + Pinecone — rodando no device do usuário.
- Embeddings multilíngues reais via transformers.js + vector store por cosseno, salvo como JSON. Zero chave de API.
- **Validado, não prometido:** uma pergunta em português quase sem palavras em comum ranqueia o trecho certo em 1º (score 0,84); 7/7 testes automatizados.
- Honesto por design: traz um fallback não-semântico rotulado para testes offline e se recusa a fingir que é o retriever.

### Como eu construo (a disciplina)
Cada módulo segue o mesmo gate: fazer → validar → testar → lapidar → corrigir → revalidar → docs bilíngues → publicar. A S1 pegou um problema real de qualidade (modelo em inglês ficava fraco em português), que corrigi trocando para o multilingual-e5 — exatamente para isso serve a esteira.

### Innovation lens
**Horizonte 3 — opção estratégica.** Spinout da camada de infra do Lastro virando toolkit OSS público. Três retornos ao mesmo tempo: (1) corta CAC de infra dos meus produtos em 80–100%; (2) constrói marca e confiança de developer; (3) cria opcionalidade para ofertas comerciais futuras (managed-hosting, suporte enterprise).

### AI Engineering decisions
| Decisão | O que escolhi | Por quê |
|---|---|---|
| **Benchmark honesto, não marketing** | Todo módulo entrega `BENCHMARK.md` vs. o unicórnio que substitui; assume os 80% que ganha, é transparente sobre os 20% que não | Confiança é o produto; mentir pro usuário é o jeito mais rápido de matar OSS |
| **Multilíngue primeiro** | `multilingual-e5-large` em vez de modelos só em inglês | Usuário PT/ES tinha recuperação ruim — eval pegou, corrigi |
| **Híbrido BM25 + denso + RRF + MMR em `@nokey/rag`** | Recuperação híbrida bate embeddings ingênuos em qualidade e robustez a query por palavra-chave | A $0 vs. Pinecone, ganha duplo |
| **Demos no navegador** (pdf.js, WebLLM, Web Speech) | Menos atrito — usuário testa antes de instalar | Adoção OSS é dor de atrito |
| **Testado, não slide** | ~39 testes automatizados em 6 módulos; gate no PR | "Funciona na minha máquina" não sobrevive ao contato com usuário |

### Por que importa
Mostra que entrego **infraestrutura real e testada** (não slide), faço trade-offs honestos de engenharia e entendo a dor de custo/lock-in que todo builder sente.
