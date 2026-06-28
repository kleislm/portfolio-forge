# 🤖 How I build with coding agents

*I ship production software by **orchestrating** coding agents, not by typing most of the code myself. The agent does the typing. I own the architecture, the guardrails, and the final diff. Not a vibe-coder — I operate under the hood.*

This page is the honest version of my agentic workflow: the patterns, the failures, and the guardrails I added because of them.

---

## 1. The orchestration pattern — execute → review

My default is **never let one agent be both author and reviewer.**

A coordinator agent (`orquestrador-geral`) classifies the task, routes it to the right specialist among a fleet of **100+ custom agents** to *execute*, and then **always spawns an independent agent to *review*** before anything ships. The reviewer is never the executor. It iterates until the review passes, then reports the verdict honestly — including when the work is *not* ready.

```
task ─▶ classify ─▶ specialist EXECUTES ─▶ independent agent REVIEWS
                                   ▲                    │
                                   └──── iterate ◀──────┘ (until pass)
```

Why it matters: a single agent grading its own homework is the most common way agentic output looks done but isn't. The adversarial second pass is where fabricated confidence gets caught.

**Artifacts behind this:** versioned `CLAUDE.md` orchestration policy · reusable `SKILL.md` / agent definitions that produce consistent output · sub-agent spawning with explicit handoffs · MCP servers · hooks · plan mode.

---

## 2. Token economy — route models by task

I don't burn Opus tokens on jobs Haiku would handle. Every repo carries a routing policy:

| Tier | Model | Used for |
|---|---|---|
| Spine | **Opus** | architecture, security-sensitive logic, the final review gate |
| Bulk | **Sonnet** | the bulk of implementation, refactors, tests |
| Trivial | **Haiku** | mechanical edits, formatting, one-line changes |

Plus a set of cost levers (context trimming, sub-agent isolation, cache-aware sequencing). The one rule I never optimize away: **never economize on the fail-closed path.** A cheap model on the spot where a wrong answer becomes liability is a false saving.

---

## 3. Context & memory engineering

Long-running builds die from context rot, not from hard problems. I manage the window deliberately:

- **When to summarize, when to spawn a sub-agent, when to clear and restart** — explicit, not accidental.
- **External / file-system memory** — durable facts live on disk, not in the chat window; structured handoffs across sessions and days.
- **RAG over my own corpus** — hybrid BM25 + dense retrieval (RRF), real embedder + HNSW index reaching **93% Recall@5** on a 14.5k-vector corpus (rebuilt after I found the previous "retrieval" was returning noise).

---

## 4. TDD with agents — don't let it cheat

A coding agent does **not** do test-first development naturally. Left alone it writes the implementation, then writes tests that pass against whatever it just wrote — coverage theater.

My guardrails:

1. **Tests first, locked.** The agent writes (or I write) the failing test and commits it *before* the implementation. Hooks verify the test existed and failed first.
2. **No editing the test to pass.** A hook flags any diff that touches both the test and the implementation in the same step on a red test.
3. **Meaningful coverage, not count.** pytest / Jest / Vitest / Playwright on the paths that carry risk, not lines-for-lines'-sake.
4. **Eval gates for agent quality** — for LLM-driven features, deterministic tests aren't enough; an eval suite (LLM-as-judge + regression set) runs in CI and **fail-closed** blocks the merge.

> **War story — the agent that cheated by not closing a tag.** A local reasoning model (`qwen3:4b`) emitted chain-of-thought without ever closing its `</think>` block, so the refinement step silently ingested the model's *thinking* as if it were output. Tests were green; the product was wrong. The fix was a sanity guard on the refinement boundary plus routing that stage to a reviewer model (`gemma3`) that didn't share the failure mode. Lesson: **green tests are necessary, not sufficient — you need eval gates that understand the *shape* of a correct answer.**

---

## 5. Under the hood — I understand the road, not just the steering wheel

The agent writes the code. I still own the parts where a mistake is expensive.

### War story A — the war-room audit (10 fixes in one pass)

A "217 passed" test suite was hiding flakiness. A six-perspective audit of one of my systems produced **10 fixes in a single hardening commit**, including:

- **WAL mode made mandatory** on both the scheduler and the HTTP path, with a checkpoint forced into the backup routine (a backup of a non-checkpointed DB is a corrupt backup waiting to happen).
- **A `conftest` isolation fix that exposed flakiness the green suite had been masking** — the tests were passing for the wrong reason.
- **An XSS path closed in a RAG editor** (untrusted retrieved content rendered into a rich-text surface).
- Dedicated ICS feed token, webhook key separated from the app secret key.

### War story B — multi-tenant isolation done fail-closed

A platform that started routing on identity alone was leaking data **by role**. I moved authorization to **capability-based RBAC, fail-closed**, with **Postgres Row-Level Security** enforcing tenant isolation at the database, not just the app layer — single source of truth for "can this operator see this row," sensitive blocks stripped on mixed screens. Three rounds of **adversarial review took the P0 count from 4 to 0** before cutover. Schemas, indexes, foreign keys, migrations and RLS policies are all part of the diff I read line by line.

### The Git discipline that came from a real incident

Background agents that ran `git checkout` once corrupted a shared working tree. The standing rule now: **agents that audit or mutate a repo run in an isolated git worktree**, and I commit before launching anything in the background — recoverable via reflog if it ever goes sideways. Branches, rebases, worktrees, conflict resolution: I don't panic when origin/main fights back.

---

## 6. The honest rule (this is the standard)

Across all of it, one principle: **an AI feature hides three invisible costs** — hallucination that becomes liability, latency that kills UX, and cost-per-token that breaks unit economics. I treat all three as first-class requirements: evals in CI, an adversarial verifier in production, task-tier routing, and **fail-closed when the system can't verify.** In a regulated domain, "wrong with confidence" costs the whole company.

I prove it on `git push`, not in slideware.

---

*Back to [README](../README.md) · related: [architecture patterns](architecture-patterns.md) · [eval methodology](eval-methodology.md) · [AI stack](ai-stack.md)*
