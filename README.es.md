<!-- Idioma: 🇪🇸 Español · 🇧🇷 Português → README.pt-BR.md (principal) · 🇬🇧 English → README.md -->

# 🧭⚙️ Kleist Monteiro
### Gerente de Innovación & IA · AI Engineer · Founder

*Construyo IA en dominios regulados — eval-first, fail-closed, **prueba clicable** (no slides).*

[🇧🇷 Português](README.pt-BR.md) · [🇬🇧 English](README.md) · **🇪🇸 Español**

---

### ▶︎ Prueba en vivo (real — clicables)

| Producto | Dónde | Qué verificás en 30s |
|---|---|---|
| **Assist4Doc** ⭐ | [assist4doc.com](https://assist4doc.com) | Grabás 10s · se vuelve SOAP estructurado · el médico aprueba o nada va a la historia clínica |
| **NoKey** (OSS) | [github.com/kleislm/nokey](https://github.com/kleislm/nokey) | Abrí cualquier demo · DevTools → Network: **cero llamadas a API** después de cargar el modelo |
| **Pamella's Hub** | [pamellamonteiro.com](https://pamellamonteiro.com) | Portfolio + CMS + *lead scoring* con LLM |
| **Staff Forge** | [pmforge.com.br](https://pmforge.com.br) | Constructor de *eval suite* · LLM-as-judge · herramientas hands-on de AI PM |
| **Lastro** ⭐ | (stealth) | Mirá [`engineering/architecture-patterns.md`](engineering/architecture-patterns.md) — entendé por qué un escrito jurídico con cita inventada **nunca sale** |

## Por qué

La innovación en IA esconde tres cosas: **(1)** el gerente solo conduce reuniones y no ve el ROI evaporarse en latencia y *prompt rework*; **(2)** el ingeniero solo codea y no ve horizonte 2/3 ni gobernanza; **(3)** casi nadie tiene la fluencia regulatoria para poner IA en salud, derecho y finanzas sin generar pasivo.

Yo cubro los tres — y lo pruebo en el `git push`.

## Trabajos seleccionados

| Producto | El dolor que mata 💔 | Horizonte | AI stack | En vivo | Caso |
|---|---|---|---|:--:|:--:|
| **Lastro** ⭐ | Escrito jurídico con cita inventada = pasivo profesional grave | **H2** — nuevo *operating model* para la práctica jurídica | LangGraph · verified RAG · panel multi-IA · OpenFGA | stealth | [📖](case-studies/lastro.md) |
| **Assist4Doc** ⭐ | Médicos pierden 3h/día en historias clínicas; nota incompleta = riesgo clínico-jurídico | **H1** a escala — productividad regulada | STT + LLM con *grounding* · humano-en-loop · multi-tenant | [↗](https://assist4doc.com) | [📖](case-studies/assist4doc.md) |
| **NoKey** (OSS) | $$$ en APIs pagas que son sólo *compute* | **H3** — opción vía plataforma | transformers.js · WebGPU · faster-whisper · Playwright · pdf.js | [↗](https://github.com/kleislm/nokey) | [📖](case-studies/nokey.md) |
| **Pamella's Hub** | Creadora sin CRM perdiendo leads que ni sabía que eran leads | **H1** — IA en B2C | *Lead scoring* con LLM · embeddings de perfil · CMS headless | [↗](https://pamellamonteiro.com) | [📖](case-studies/pamella-ai-hub.md) |
| **Staff Forge** | PM volviéndose AI PM sin práctica hands-on | Capacitación | LLM-as-judge · dataset curado · regression suite | [↗](https://pmforge.com.br) | [📖](case-studies/staff-forge.md) |

> **+9 productos** en el [índice completo (14)](case-studies/_index.md): TCU Navigator · KLM Legal Hub · ConversaFlow · Insight Health · Agenda Connect · Leone · Desburocratize · OrdemXP · Modulazzi.

## Galería — capturas reales

<sub>*Capturas reales de los sistemas en producción, no mockups.*</sub>

<table>
<tr>
<td width="33%" valign="top"><a href="case-studies/assist4doc.md"><img src="assets/assist4doc.png" width="100%"/></a><br/><b>Assist4Doc ⭐</b><br/><sub><a href="https://assist4doc.com">assist4doc.com</a> · documentación clínica IA</sub></td>
<td width="33%" valign="top"><a href="case-studies/pamella-ai-hub.md"><img src="assets/pamella-ai-hub.png" width="100%"/></a><br/><b>Pamella's Hub</b><br/><sub><a href="https://pamellamonteiro.com">pamellamonteiro.com</a> · CMS + CRM con IA</sub></td>
<td width="33%" valign="top"><a href="case-studies/staff-forge.md"><img src="assets/staff-forge.png" width="100%"/></a><br/><b>Staff Forge / PM Forge</b><br/><sub><a href="https://pmforge.com.br">pmforge.com.br</a> · evals + guardrails</sub></td>
</tr>
<tr>
<td width="33%" valign="top"><a href="case-studies/klm-legal-hub.md"><img src="assets/klm-legal-hub.png" width="100%"/></a><br/><b>KLM Legal Hub</b><br/><sub><a href="https://klmadvogados.com">klmadvogados.com</a> · gestión de casos</sub></td>
<td width="33%" valign="top"><a href="case-studies/leone-growth.md"><img src="assets/leone-growth.png" width="100%"/></a><br/><b>Leone Growth</b><br/><sub><a href="https://leone.lovable.app">leone.lovable.app</a> · CRO US</sub></td>
<td width="33%" valign="top"><a href="case-studies/desburocratize.md"><img src="assets/desburocratize.png" width="100%"/></a><br/><b>Desburocratize</b><br/><sub><a href="https://desburocratize.lovable.app">desburocratize.lovable.app</a> · branding</sub></td>
</tr>
</table>

## La regla honesta (este es el moat)

Una feature de IA esconde **tres costos invisibles**: alucinación que se vuelve pasivo, latencia que mata UX, y *cost-per-token* que rompe la *unit economics*. Trato los tres como requisitos de primera clase — **evals en CI**, verificador adversarial en producción, ruteo por *tier* de tarea, **fail-closed** cuando el sistema no puede verificar. En dominio regulado, "equivocado con confianza" cuesta la empresa entera.

## Stack

- **Modelos** — OpenAI · Claude · Gemini · Ollama (gemma3, qwen3, llama3) · WebLLM
- **Orquestación** — LangGraph · agentes con *verifier loop* · ruteo Opus/Sonnet/Haiku por costo×calidad
- **RAG** — híbrido BM25 + denso (RRF) · MMR · verificador adversarial · banco de tesis
- **Backend** — FastAPI · APScheduler/arq · PostgreSQL + pgvector · WebSockets · OpenFGA + OPA · Zitadel
- **Frontend** — React 19 · Vite · TypeScript · Tailwind · shadcn/ui · SSR donde paga conversión
- **Evals & Guardrails** — panel multi-IA · LLM-as-judge · regression suite en CI · *fail-closed* · costo/latencia budget
- **Infra** — Docker · Supabase Edge · *blue-green deploy* · *restore test* mensual · OTel tracing

Detalle: [`engineering/ai-stack.md`](engineering/ai-stack.md) · [`eval-methodology.md`](engineering/eval-methodology.md) · [`architecture-patterns.md`](engineering/architecture-patterns.md).

## Cómo leerme

- 🧭 [Playbook de Innovación](innovation/playbook.md) — H1/H2/H3 · gobernanza · IA responsable
- 🧪 [Metodología de eval](engineering/eval-methodology.md) — panel multi-IA · LLM-as-judge · regression
- 🏗️ [Patrones de arquitectura](engineering/architecture-patterns.md) — verified RAG · verifier loop · fail-closed
- 🗺️ [Mapa de competencias](about/competencies.md) — Mehta PM (12) + AI Engineering (12)
- 👤 [Bio & tesis](about/bio.md) — 8 principios · dónde quiero operar
- 🎓 [Certificaciones](about/certifications.md) — trío IBM Product + AI Product + Business Analyst

---

**Derecho → growth → fundar → construir IA.** Brasília, BR · ✉️ kleistfilho@gmail.com · 🐙 [github.com/kleislm](https://github.com/kleislm)
*MIT-spirit portfolio. Built by Kleist Monteiro.*
