# Security Policy

🇧🇷 Português · [🇬🇧 English ↓](#-english)

Este é um **portfólio pessoal** — não roda código de runtime, não recebe entrada de terceiros, não trata dados de cliente. Mesmo assim, levo segurança a sério.

## Escopo
- Conteúdo em Markdown deste repositório
- Assets visuais publicados em `/assets`
- Possíveis trechos de código de exemplo em arquivos `engineering/*.md` (são ilustrativos, não executáveis)

## Conformidade
- **LGPD (Lei 13.709/2018)** — nenhum dado pessoal de cliente, terceiro ou colaborador é tratado neste repositório. Casos descritos em `case-studies/` foram **deliberadamente anonimizados**.
- **OAB Provimento 205/2021** — publicidade profissional respeita os limites do Estatuto da Advocacia. Nenhum cliente é identificado; nenhum resultado é prometido.
- **Sigilo profissional (EAOAB art. 7º XIX)** — observado em todos os cases jurídicos.

## Vulnerabilidades

Se você encontrar **qualquer**:
- Dado pessoal ou identificável de cliente não anonimizado
- Vazamento de credencial, token, chave ou secret
- Vulnerabilidade técnica nas integrações (ex.: Dependabot)
- Conteúdo em violação à LGPD ou ao Provimento OAB 205/2021

**Reporte privadamente** via GitHub Private Vulnerability Reporting:
1. Acesse [https://github.com/kleislm/portfolio-forge/security/advisories/new](https://github.com/kleislm/portfolio-forge/security/advisories/new)
2. Descreva o achado com clareza
3. Aguarde resposta em até 72h úteis

Para casos urgentes (vazamento ativo de credencial): **kleistfilho@gmail.com** com assunto `[SECURITY] portfolio-forge`.

**Não abra Issue pública para vulnerabilidade.**

## Proteções ativas neste repositório
- Private vulnerability reporting
- Dependency graph + Dependabot alerts + Dependabot security updates
- Secret scanning + push protection
- Copilot Autofix (CodeQL alerts)

## Compromisso
Após o report, eu:
1. Confirmo recebimento em até 72h úteis
2. Sanitizo / corrijo o conteúdo
3. Reescrevo o histórico do git se a remoção exigir limpeza retroativa
4. Atualizo este documento se a vulnerabilidade revelar gap de processo

---

## 🇬🇧 English

This is a **personal portfolio** — it runs no runtime code, accepts no third-party input, and processes no client data. Even so, I take security seriously.

## Scope
- Markdown content in this repository
- Visual assets published under `/assets`
- Illustrative (non-executable) code snippets in `engineering/*.md`

## Compliance
- **Brazilian LGPD (Law 13.709/2018)** — no personal data of clients, third parties, or collaborators is processed here. Cases in `case-studies/` are **deliberately anonymized**.
- **OAB Provimento 205/2021** (Brazilian Bar Association advertising rules) — respected throughout. No client is identified; no result is promised.
- **Professional confidentiality (Brazilian Bar Statute art. 7 XIX)** — observed in all legal cases.

## Reporting a vulnerability

If you find **any**:
- Personal or identifiable client data that wasn't anonymized
- Leak of credential, token, key, or secret
- Technical vulnerability in integrations (e.g., Dependabot)
- Content violating Brazilian LGPD or OAB rules

**Report privately** via GitHub Private Vulnerability Reporting:
1. Go to [https://github.com/kleislm/portfolio-forge/security/advisories/new](https://github.com/kleislm/portfolio-forge/security/advisories/new)
2. Describe the finding clearly
3. Expect a response within 72 business hours

For urgent cases (active credential leak): **kleistfilho@gmail.com** with subject `[SECURITY] portfolio-forge`.

**Do not open a public Issue for a vulnerability.**

## Active protections on this repository
- Private vulnerability reporting
- Dependency graph + Dependabot alerts + Dependabot security updates
- Secret scanning + push protection
- Copilot Autofix (CodeQL alerts)

## Commitment
Upon report I will:
1. Confirm receipt within 72 business hours
2. Sanitize / fix the content
3. Rewrite git history if removal requires retroactive cleanup
4. Update this document if the vulnerability reveals a process gap
