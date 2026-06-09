# Assist4Doc — AI clinical documentation ⭐

![Assist4Doc](../assets/assist4doc.png)

🇬🇧 English · [🇧🇷 Português](#-português)

**Role:** Founder · Product Manager · Builder &nbsp;|&nbsp; **Status:** Live &nbsp;|&nbsp; **Domain:** Healthtech (regulated)
🔗 [assist4doc.com](https://assist4doc.com)

### Context
Physicians spend a disproportionate share of their day on documentation. Every minute in the chart is a minute away from the patient — and incomplete notes create both clinical and legal exposure.

### Problem
Turn the spoken consultation into a trustworthy structured clinical note (SOAP), with three non-negotiables: it must not invent clinical facts, the physician must review and approve before anything counts, and the whole flow must respect privacy and auditability.

### My role & decisions
- **Positioned it as assistive, not autonomous.** The physician reviews and signs. A product decision *and* a liability shield — the human stays accountable for every clinical call.
- **Treated trustworthiness as a first-class requirement,** not a detail — structured output is checked against official medical references, and safety alerts surface automatically.
- **Designed for clinics from day one** — multi-tenant, role-based access, audit trail.
- **Built for the real world** — works on unreliable connections; multilingual.

### Innovation lens
**Horizon 1 at scale** — assistive AI on the core medical workflow, no reinvented process. Productivity gain is the metric; physician adoption is the gate. Governance baked in: human-in-the-loop, audit trail, LGPD by design.

### AI Engineering decisions
| Decision | What I chose | Why |
|---|---|---|
| **Anti-hallucination on clinical content** | Adversarial verifier checks every prescription/dose against official sources; out-of-range triggers auto-alert | A made-up dosage is a malpractice claim |
| **Human-in-the-loop as architecture, not policy** | The system **cannot** ship a record without physician signature | Liability and trust are product features, not legalese |
| **Multi-tenant from day 1 (OpenFGA-style)** | Clinic → role → patient scope, fine-grained | Selling to clinics later requires it; bolt-on is painful |
| **Works on unreliable connections** | Local capture + sync queue + offline-first UI | Real clinics, real internet — degrades gracefully |
| **STT + LLM with clinical grounding** | Whisper-class STT → structured SOAP via constrained generation | Free-form summary is unsafe; SOAP is auditable |

### Impact
- Live product running the full loop: capture → structured draft → physician review → approved record.
- Value proposition: *less time on paperwork, more time on patients.*

### Why this case defines me as a PM
It combines the three things few PMs hold at once: **AI product judgment** (evaluation, anti-hallucination, human-in-the-loop), **regulated-domain fluency** (privacy, medical compliance), and **founder-grade execution** (zero to a live, multi-tenant product).

> Internal architecture and proprietary methods are intentionally omitted from this portfolio.

---

## 🇧🇷 Português

**Papel:** Founder · Product Manager · Builder &nbsp;|&nbsp; **Status:** No ar &nbsp;|&nbsp; **Domínio:** Healthtech (regulado)
🔗 [assist4doc.com](https://assist4doc.com)

### Contexto
Médicos gastam uma fatia desproporcional do dia documentando. Cada minuto no prontuário é um minuto longe do paciente — e nota incompleta gera risco clínico e jurídico.

### Problema
Transformar a consulta falada em um prontuário estruturado e confiável (SOAP), com três inegociáveis: não inventar dados clínicos, exigir revisão e aprovação do médico antes de qualquer coisa valer, e respeitar privacidade e auditabilidade.

### Meu papel & decisões
- **Posicionei como assistivo, não autônomo.** O médico revisa e assina. Decisão de produto *e* blindagem jurídica — o humano segue responsável por toda decisão clínica.
- **Tratei a confiabilidade como requisito de primeira classe,** não detalhe — a saída estruturada é checada contra referências médicas oficiais, e alertas de segurança surgem automaticamente.
- **Pensei em clínicas desde o dia 1** — multi-tenant, acesso por perfil, trilha de auditoria.
- **Construí para o mundo real** — funciona com internet instável; multilíngue.

### Innovation lens
**Horizonte 1 em escala** — IA assistiva sobre o fluxo médico atual, sem reinventar processo. Métrica é ganho de produtividade; gate é adoção pelo médico. Governança embutida: humano-no-loop, trilha de auditoria, LGPD by design.

### AI Engineering decisions
| Decisão | O que escolhi | Por quê |
|---|---|---|
| **Anti-alucinação em conteúdo clínico** | Verificador adversarial confere medicamento/dose contra base oficial; fora de faixa dispara alerta auto | Dose inventada é processo por imperícia |
| **Humano-no-loop como arquitetura, não política** | Sistema **não consegue** entregar prontuário sem assinatura médica | Responsabilidade e confiança são features, não letras miúdas |
| **Multi-tenant desde o dia 1 (estilo OpenFGA)** | Clínica → papel → escopo de paciente, fine-grained | Vender a clínica depois exige isso; remendo dói |
| **Funciona com internet ruim** | Captura local + fila de sync + UI offline-first | Clínicas reais têm internet ruim — degrada graciosamente |
| **STT + LLM com *grounding* clínico** | STT classe-Whisper → SOAP estruturado via geração restrita | Resumo livre é inseguro; SOAP é auditável |

### Impacto
- Produto no ar rodando o ciclo completo: captura → rascunho estruturado → revisão médica → prontuário aprovado.
- Proposta de valor: *menos tempo com burocracia, mais tempo com pacientes.*

### Por que este caso me define como PM
Reúne as três coisas que poucos PMs têm ao mesmo tempo: **julgamento de produto de IA** (avaliação, anti-alucinação, humano no loop), **fluência em domínio regulado** (privacidade, compliance médico) e **execução de founder** (do zero a um produto no ar, multi-tenant).

> Arquitetura interna e métodos proprietários foram omitidos de propósito neste portfólio.
