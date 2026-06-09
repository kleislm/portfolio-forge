# Architecture patterns — verified RAG, verifier loop, fail-closed

🇧🇷 Português · [🇬🇧 English ↓](#-english)

Padrões que apliquei em produção. Cada um nasceu de uma dor real e está em pelo menos um dos cases.

## 1. RAG Verificado (Verified RAG)

**Problema:** RAG clássico recupera + gera. Se o passo de geração inventa citação (ex.: REsp inexistente), ninguém percebe — o usuário aceita porque "veio com fonte".

**Solução:** verificação adversarial pós-geração.

```
┌────────┐    ┌──────────────┐   ┌──────────┐   ┌────────────────────┐
│ Query  │───►│ Hybrid       │──►│ LLM      │──►│ External Verifier  │
│        │    │ retrieve     │   │ generate │   │ (Jusbrasil/STJ/    │
│        │    │ (BM25+dense  │   │ + cite   │   │  Anvisa/DataJud)   │
└────────┘    │  + RRF + MMR)│   └──────────┘   └─────────┬──────────┘
                                                          │
                                  ┌───────────────────────┴────────┐
                                  ▼                                ▼
                          ✅ all citations valid           ❌ at least one fake
                                  │                                │
                                  ▼                                ▼
                          render to user                  FAIL-CLOSED:
                                                          "needs human review"
```

**Onde aplico:** Lastro (peticionamento), NoKey (RAG OSS).
**Custo extra:** ~1 chamada de verificação por citação, paralelizada. Caro? Sim. Mas a peça com REsp inventado gera passivo profissional grave para quem assina.

## 2. Verifier Loop (agente que se valida antes de entregar)

**Problema:** Agente LLM entrega resposta "plausível" mas errada. Sem checagem, vai para produção.

**Solução:** loop interno onde o próprio grafo do agente tem nó "verificador" obrigatório.

```
            ┌───────────┐
            │  Planner  │
            └─────┬─────┘
                  ▼
            ┌───────────┐
       ┌───►│  Worker   │
       │    └─────┬─────┘
       │          ▼
       │    ┌───────────┐    fail
       │    │ Verifier  ├──────┐  (max 3 retries)
       │    └─────┬─────┘      │
       │          │ pass       │
       │          ▼            │
       │    ┌───────────┐      │
       │    │  Output   │      │
       │    └───────────┘      │
       └────────────────────────┘
```

**Implementação:** LangGraph. O **verifier** é outro modelo (diversidade de erro) com rubrica explícita.
**Onde aplico:** Lastro (geração de peça → verifier de jurisprudência), Assist4Doc (SOAP → verifier clínico).

## 3. Painel Multi-IA para seleção de modelo

**Problema:** "Qual modelo é melhor para minha tarefa?" — a resposta padrão é benchmark genérico (MMLU, HumanEval). Inútil para domínio específico.

**Solução:** painel que roda **a mesma tarefa real** em N modelos, juízes votam, eu vejo o vencedor por tarefa.

| Tarefa | Modelo vencedor (eval interno) | Custo relativo |
|---|---|---|
| Triagem de email jurídico | Haiku | 1× |
| Sumarização de processo (100 pág) | Sonnet | 3× |
| Geração de peça crítica | Opus | 10× |
| Drafter rápido (interno) | Ollama `gemma3:4b` local | ~0× |
| Verificador adversarial | Claude Sonnet + Gemini 2.5 Pro (painel) | 4× |

**Onde aplico:** Lastro (decisão de roteamento), eval suite (LLM-as-judge).

## 4. Fail-Closed

**Problema:** Sistema continua entregando mesmo quando suas próprias verificações falham — para "não travar UX".

**Solução:** se o verificador falha → **para**. Devolve mensagem clara: "preciso de revisão humana antes de seguir".

```python
def generate_brief(case):
    draft = llm.generate(prompt(case))
    citations = extract_citations(draft)
    verified = [jusbrasil.verify(c) for c in citations]
    if not all(verified):
        raise FailClosed(
            reason="citation_unverified",
            user_message="Não consegui validar 1 ou mais precedentes. Peça revisão humana antes do protocolo.",
            artifact=draft,  # mostro o draft para o humano analisar
        )
    return draft
```

**Filosofia:** prefiro entregar "**falhei e por isso**" do que "**entreguei algo errado com confiança**". Em domínio regulado, segundo é fim de carreira.

## 5. Cost × Quality Routing (Max 20x)

**Problema:** rodar tudo em Opus quebra unit economics; rodar tudo em Haiku quebra qualidade na decisão crítica.

**Solução:** classificar tarefa em `tier ∈ {trivial, bulk, backbone}`. Roteador escolhe.

```python
ROUTING = {
    "trivial":  {"primary": "haiku",   "max_cost_usd": 0.001},
    "bulk":     {"primary": "sonnet",  "max_cost_usd": 0.05},
    "backbone": {"primary": "opus",    "max_cost_usd": 0.30},
}

def route(task):
    cfg = ROUTING[task.tier]
    if estimate_cost(task, cfg["primary"]) > cfg["max_cost_usd"]:
        return downgrade(task)  # tier abaixo, com log
    return cfg["primary"]
```

**Regra de ouro:** **nunca economizar no fail-closed.** O verificador é sempre tier `backbone` — o erro ali é o que destrói o produto.

## 6. Multi-tenant authz fine-grained (OpenFGA + OPA)

**Problema:** "advogado A não pode ver pasta do cliente B do sócio C" — RBAC tradicional não modela bem.

**Solução:** **OpenFGA** (relacional, estilo Zanzibar) para "quem pode o quê em qual recurso" + **OPA** para política de escopo de feature.

Modelo de exemplo:
```
type pasta
  relations
    define owner: [user]
    define collaborator: [user] or owner
    define viewer: [user] or collaborator
```

E a query: `check(user="ana", relation="viewer", object="pasta:<process-id>")`.

**Onde aplico:** Lastro (multi-sócio + multi-cliente + escopo de pasta).

## 7. Durable timer (APScheduler / arq)

**Problema:** task longa dentro do request (peça de 200 páginas, transcrição de 1h) trava o handler.

**Solução:** request enfileira; worker durável processa; WebSocket entrega progresso.

```
HTTP POST /generate ──► enqueue(task) ──► return {task_id, ws_url}
                                    │
                                    ▼
                            APScheduler worker
                                    │
                                    ▼
                            ws.send(progress) ──► UI atualiza
```

**Onde aplico:** Lastro (peça longa), Assist4Doc (transcrição + SOAP).

## 8. Blue-green deploy + restore test mensal

**Problema:** deploy com migração de banco em produto crítico = potencial para *down* total.

**Solução:** blue-green + teste de restore mensal — **provo** que sei voltar.

- Deploy: novo container sobe paralelo, smoke tests, *swap* atômico.
- Restore test: 1× por mês, *snapshot* mais recente do prod é restaurado em ambiente isolado e validado.

Sem o teste mensal, "**eu tenho backup**" é fé, não fato.

---

## 🇬🇧 English

Production-tested patterns. Each was born from a real pain and lives in at least one of my cases.

## 1. Verified RAG

**Problem:** classic RAG retrieves + generates. If generation invents a citation (e.g., a nonexistent case-law number), no one notices — the user trusts the "with source" label.

**Solution:** adversarial post-generation verification.

```
┌────────┐    ┌──────────────┐   ┌──────────┐   ┌────────────────────┐
│ Query  │───►│ Hybrid       │──►│ LLM      │──►│ External Verifier  │
│        │    │ retrieve     │   │ generate │   │ (Jusbrasil/STJ/    │
│        │    │ (BM25+dense  │   │ + cite   │   │  Anvisa/DataJud)   │
└────────┘    │  + RRF + MMR)│   └──────────┘   └─────────┬──────────┘
                                                          │
                                  ┌───────────────────────┴────────┐
                                  ▼                                ▼
                          ✅ all citations valid           ❌ at least one fake
                                  │                                │
                                  ▼                                ▼
                          render to user                  FAIL-CLOSED:
                                                          "needs human review"
```

**Where I apply it:** Lastro (brief generation), NoKey (OSS RAG).
**Extra cost:** ~1 verification call per citation, parallelized. Expensive? Yes. But a brief with a fake citation costs the entire firm's liability.

## 2. Verifier Loop (agent self-validates before shipping)

**Problem:** an LLM agent ships a "plausible" but wrong answer. Without a check, it reaches production.

**Solution:** internal loop where the agent's own graph has a mandatory "verifier" node.

```
            ┌───────────┐
            │  Planner  │
            └─────┬─────┘
                  ▼
            ┌───────────┐
       ┌───►│  Worker   │
       │    └─────┬─────┘
       │          ▼
       │    ┌───────────┐    fail
       │    │ Verifier  ├──────┐  (max 3 retries)
       │    └─────┬─────┘      │
       │          │ pass       │
       │          ▼            │
       │    ┌───────────┐      │
       │    │  Output   │      │
       │    └───────────┘      │
       └────────────────────────┘
```

**Implementation:** LangGraph. The **verifier** is a different model (error diversity) with an explicit rubric.
**Where I apply it:** Lastro (brief generation → case-law verifier), Assist4Doc (SOAP → clinical verifier).

## 3. Multi-AI Panel for model selection

**Problem:** "Which model is best for my task?" — the default answer is a generic benchmark (MMLU, HumanEval). Useless for a specific domain.

**Solution:** a panel that runs **the same real task** in N models; judges vote; I see the winner per task.

| Task | Winning model (internal eval) | Relative cost |
|---|---|---|
| Legal email triage | Haiku | 1× |
| Process summary (100 pages) | Sonnet | 3× |
| Critical brief generation | Opus | 10× |
| Fast internal drafter | Local Ollama `gemma3:4b` | ~0× |
| Adversarial verifier | Claude Sonnet + Gemini 2.5 Pro (panel) | 4× |

**Where I apply it:** Lastro (routing decisions), eval suite (LLM-as-judge).

## 4. Fail-Closed

**Problem:** a system keeps shipping even when its own checks fail — "to not block UX."

**Solution:** if the verifier fails → **stop**. Return a clear message: "I need human review before proceeding."

```python
def generate_brief(case):
    draft = llm.generate(prompt(case))
    citations = extract_citations(draft)
    verified = [jusbrasil.verify(c) for c in citations]
    if not all(verified):
        raise FailClosed(
            reason="citation_unverified",
            user_message="I couldn't validate 1+ precedents. Please review manually before filing.",
            artifact=draft,  # show the draft so the human can analyze
        )
    return draft
```

**Philosophy:** I'd rather deliver "**I failed, here's why**" than "**I delivered something wrong with confidence**." In regulated domains, the second is career-ending.

## 5. Cost × Quality Routing (Max 20x)

**Problem:** running everything on Opus breaks unit economics; running everything on Haiku breaks quality on critical decisions.

**Solution:** classify the task into `tier ∈ {trivial, bulk, backbone}`. Router picks.

```python
ROUTING = {
    "trivial":  {"primary": "haiku",   "max_cost_usd": 0.001},
    "bulk":     {"primary": "sonnet",  "max_cost_usd": 0.05},
    "backbone": {"primary": "opus",    "max_cost_usd": 0.30},
}

def route(task):
    cfg = ROUTING[task.tier]
    if estimate_cost(task, cfg["primary"]) > cfg["max_cost_usd"]:
        return downgrade(task)  # tier below, with log
    return cfg["primary"]
```

**Golden rule:** **never economize on fail-closed.** The verifier is always `backbone` tier — the error there is what kills the product.

## 6. Fine-grained multi-tenant authz (OpenFGA + OPA)

**Problem:** "lawyer A can't see partner C's client B's folder" — traditional RBAC doesn't model this well.

**Solution:** **OpenFGA** (relational, Zanzibar-style) for "who can do what on which resource" + **OPA** for feature scope.

Example model:
```
type folder
  relations
    define owner: [user]
    define collaborator: [user] or owner
    define viewer: [user] or collaborator
```

And the query: `check(user="ana", relation="viewer", object="folder:<process-id>")`.

**Where I apply it:** Lastro (multi-partner + multi-client + folder scope).

## 7. Durable timer (APScheduler / arq)

**Problem:** a long task inside the request (200-page brief, 1h transcription) blocks the handler.

**Solution:** request enqueues; durable worker processes; WebSocket delivers progress.

```
HTTP POST /generate ──► enqueue(task) ──► return {task_id, ws_url}
                                    │
                                    ▼
                            APScheduler worker
                                    │
                                    ▼
                            ws.send(progress) ──► UI updates
```

**Where I apply it:** Lastro (long briefs), Assist4Doc (transcription + SOAP).

## 8. Blue-green deploy + monthly restore test

**Problem:** a deploy with DB migration on a critical product = potential total downtime.

**Solution:** blue-green + monthly restore test — I **prove** I can roll back.

- Deploy: new container alongside, smoke tests, atomic swap.
- Restore test: 1×/month, the most recent prod snapshot is restored to an isolated env and validated.

Without the monthly test, "**I have backups**" is faith, not fact.
