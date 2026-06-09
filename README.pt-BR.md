<!-- Idioma: 🇧🇷 Português (principal) · 🇬🇧 English → README.md · 🇪🇸 Español → README.es.md -->

# 🧭⚙️ Kleist Monteiro
### Gerente de Inovação & IA · AI Engineer · Founder

*Construo IA em domínio regulado — eval-first, fail-closed, **prova clicável** (não slide).*

**🇧🇷 Português** · [🇬🇧 English](README.md) · [🇪🇸 Español](README.es.md)

---

### ▶︎ Prova ao vivo (real — clicáveis)

| Produto | Onde | O que você confere em 30s |
|---|---|---|
| **Assist4Doc** ⭐ | [assist4doc.com](https://assist4doc.com) | Grava 10s · vira SOAP estruturado · médico aprova ou nada vai pra prontuário |
| **NoKey** (OSS) | [github.com/kleislm/nokey](https://github.com/kleislm/nokey) | Abra qualquer demo · DevTools → Network: **zero API calls** depois do modelo carregar |
| **Pamella's Hub** | [pamellamonteiro.com](https://pamellamonteiro.com) | Portfólio + CMS + lead scoring por LLM |
| **Staff Forge** | [pmforge.com.br](https://pmforge.com.br) | Eval suite builder · LLM-as-judge · ferramentas hands-on de AI PM |
| **Lastro** ⭐ | (stealth) | Veja [`engineering/architecture-patterns.md`](engineering/architecture-patterns.md) — entenda por que peça com REsp inventado **não sai** |

## Por quê

Inovação em IA esconde três coisas: **(1)** o gerente só conduz reunião e não vê o ROI evaporar em latência e *prompt rework*; **(2)** o engenheiro só coda e não enxerga horizonte 2/3 nem governança; **(3)** quase ninguém tem fluência regulatória para botar IA em saúde, jurídico e finanças sem virar passivo.

Eu cubro os três — e provo no `git push`.

## Trabalhos selecionados

| Produto | A dor que mata 💔 | Horizonte | AI stack | Ao vivo | Caso |
|---|---|---|---|:--:|:--:|
| **Lastro** ⭐ | Peça jurídica com REsp inventado = passivo profissional grave | **H2** — novo *operating model* para advocacia | LangGraph · verified RAG · painel multi-IA · OpenFGA | stealth | [📖](case-studies/lastro.md) |
| **Assist4Doc** ⭐ | Médico perde 3h/dia em prontuário; nota incompleta = risco clínico-jurídico | **H1** escalável — produtividade regulada | STT + LLM com *grounding* · humano-no-loop · multi-tenant | [↗](https://assist4doc.com) | [📖](case-studies/assist4doc.md) |
| **NoKey** (OSS) | $$$ de API paga que é só *compute* | **H3** — opção via plataforma | transformers.js · WebGPU · faster-whisper · Playwright · pdf.js | [↗](https://github.com/kleislm/nokey) | [📖](case-studies/nokey.md) |
| **Pamella's Hub** | Criadora sem CRM, perdendo lead que nem sabia que era lead | **H1** — IA em B2C | LLM lead scoring · embeddings de perfil · CMS headless | [↗](https://pamellamonteiro.com) | [📖](case-studies/pamella-ai-hub.md) |
| **Staff Forge** | PM virando AI PM sem prática hands-on | Capacitação | LLM-as-judge · dataset curado · regression suite | [↗](https://pmforge.com.br) | [📖](case-studies/staff-forge.md) |

> **+9 produtos** no [índice completo (14)](case-studies/_index.md): TCU Navigator · KLM Legal Hub · ConversaFlow · Insight Health · Agenda Connect · Leone · Desburocratize · OrdemXP · Modulazzi.

## Vitrine — screenshots ao vivo

<sub>*Capturas reais dos sistemas em produção, não mockups.*</sub>

<table>
<tr>
<td width="33%" valign="top"><a href="case-studies/assist4doc.md"><img src="assets/assist4doc.png" width="100%"/></a><br/><b>Assist4Doc ⭐</b><br/><sub><a href="https://assist4doc.com">assist4doc.com</a> · documentação clínica IA</sub></td>
<td width="33%" valign="top"><a href="case-studies/pamella-ai-hub.md"><img src="assets/pamella-ai-hub.png" width="100%"/></a><br/><b>Pamella's Hub</b><br/><sub><a href="https://pamellamonteiro.com">pamellamonteiro.com</a> · CMS + CRM com IA</sub></td>
<td width="33%" valign="top"><a href="case-studies/staff-forge.md"><img src="assets/staff-forge.png" width="100%"/></a><br/><b>Staff Forge / PM Forge</b><br/><sub><a href="https://pmforge.com.br">pmforge.com.br</a> · evals + guardrails</sub></td>
</tr>
<tr>
<td width="33%" valign="top"><a href="case-studies/klm-legal-hub.md"><img src="assets/klm-legal-hub.png" width="100%"/></a><br/><b>KLM Legal Hub</b><br/><sub><a href="https://klmadvogados.com">klmadvogados.com</a> · gestão de casos</sub></td>
<td width="33%" valign="top"><a href="case-studies/leone-growth.md"><img src="assets/leone-growth.png" width="100%"/></a><br/><b>Leone Growth</b><br/><sub><a href="https://leone.lovable.app">leone.lovable.app</a> · CRO US</sub></td>
<td width="33%" valign="top"><a href="case-studies/desburocratize.md"><img src="assets/desburocratize.png" width="100%"/></a><br/><b>Desburocratize</b><br/><sub><a href="https://desburocratize.lovable.app">desburocratize.lovable.app</a> · branding</sub></td>
</tr>
</table>

## A regra honesta (é o moat)

Feature de IA esconde **três custos invisíveis**: alucinação que vira passivo, latência que mata UX, e *cost-per-token* que quebra *unit economics*. Eu trato os três como requisito de primeira classe — **eval no CI**, verificador adversarial em produção, roteamento por *tier* de tarefa, **fail-closed** quando não dá pra verificar. Em domínio regulado, "errado com confiança" custa a empresa toda.

## Stack

- **Modelos** — OpenAI · Claude · Gemini · Ollama (gemma3, qwen3, llama3) · WebLLM
- **Orquestração** — LangGraph · agentes com *verifier loop* · roteamento Opus/Sonnet/Haiku por custo×qualidade
- **RAG** — híbrido BM25 + denso (RRF) · MMR · verificador adversarial · banco de teses
- **Backend** — FastAPI · APScheduler/arq · PostgreSQL + pgvector · WebSockets · OpenFGA + OPA · Zitadel
- **Frontend** — React 19 · Vite · TypeScript · Tailwind · shadcn/ui · SSR onde paga conversão
- **Evals & Guardrails** — painel multi-IA · LLM-as-judge · regression suite no CI · *fail-closed* · custo/latência budget
- **Infra** — Docker · Supabase Edge · *blue-green deploy* · *restore test* mensal · OTel tracing

Detalhe: [`engineering/ai-stack.md`](engineering/ai-stack.md) · [`eval-methodology.md`](engineering/eval-methodology.md) · [`architecture-patterns.md`](engineering/architecture-patterns.md).

## Como me ler

- 🧭 [Playbook de Inovação](innovation/playbook.md) — H1/H2/H3 · governança · IA responsável
- 🧪 [Metodologia de eval](engineering/eval-methodology.md) — painel multi-IA · LLM-as-judge · regression
- 🏗️ [Padrões de arquitetura](engineering/architecture-patterns.md) — verified RAG · verifier loop · fail-closed
- 🗺️ [Mapa de competências](about/competencies.md) — Mehta PM (12) + AI Engineering (12)
- 👤 [Bio & tese](about/bio.md) — 8 princípios · onde quero atuar
- 🎓 [Certificações](about/certifications.md) — tripé IBM Product + AI Product + Business Analyst

---

**Direito → growth → fundar → construir IA.** Brasília, BR · ✉️ kleistfilho@gmail.com · 🐙 [github.com/kleislm](https://github.com/kleislm)
*MIT-spirit portfolio. Built by Kleist Monteiro.*
