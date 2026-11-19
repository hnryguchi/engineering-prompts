# Engineering Prompts

[Português](README.md) · **English**

A library of reusable prompt templates for working with AI coding agents on production-bound software: audits, phased execution with approval gates, safe deploys, spec-driven builds, and domain-expert review agents.

> Templates distilled from prompts I used while building production systems with AI coding agents (Claude Code).

The templates themselves are written in Brazilian Portuguese, the language I work in. Each one includes when to use it, the prompt text with `{{placeholders}}`, why it works, an anonymised example and common pitfalls. All examples are generic; no real client, personal or infrastructure data is included.

## Index

| # | Template | What it is for |
|---|----------|----------------|
| 01 | [Auditoria somente leitura](prompts/01-auditoria-somente-leitura.md) | Read-only audit of the current state before any change; ends in a plan awaiting approval |
| 02 | [Execução em fases com autorização](prompts/02-execucao-em-fases-com-autorizacao.md) | Autonomous execution of routine, reversible work with explicit stop points (push, deploy, data, secrets, visibility) |
| 03 | [Retomar de onde parou](prompts/03-retomar-de-onde-parou.md) | Resume an interrupted session without restarting or undoing correct work |
| 04 | [Isolamento de projeto](prompts/04-isolamento-de-projeto.md) | Hard boundaries so the agent never reads or touches other projects, accounts or cloud resources |
| 05 | [Checklist pré-deploy e migração](prompts/05-checklist-pre-deploy-e-migracao.md) | Narrow, verifiable production authorization, including a database migration |
| 06 | [Especificação completa de produto](prompts/06-especificacao-completa-de-produto.md) | Spec-driven build of a new multi-component app, with privacy rules, priorities and acceptance script |
| 07 | [Auditoria final de verificação](prompts/07-auditoria-final-de-verificacao.md) | Verification-only pass with a PASS/FAIL table; no new features |
| 08 | [Automação de planilha](prompts/08-automacao-de-planilha.md) | Spreadsheet automation: analyse first, then header-based mapping with confidence scoring and safe refusal |
| 09 | [Agente revisor especialista](prompts/09-agente-revisor-especialista.md) | System prompt: domain expert that confirms or discards deterministic findings, with source and reason |
| 10 | [Agente redator que não envia](prompts/10-agente-redator-que-nao-envia.md) | System prompt: drafting agent that re-verifies, matches the reviewer's style and only produces drafts |
| 11 | [AGENTS.md do repositório](prompts/11-agents-md-do-repositorio.md) | Durable repository rules: architecture, commands, deploy, security, "never without authorization" |
| 12 | [Dados honestos e estado vazio](prompts/12-dados-honestos-e-estado-vazio.md) | Snippet: never invent data or business rules; zero vs. missing vs. unavailable; DEMO isolation |
| 13 | [Reproduzir, corrigir, validar](prompts/13-correcao-reproduzir-corrigir-validar.md) | Bug fix that requires reproduction, root cause, no cosmetic masking, and browser evidence |

## Principles

1. **Read before write.** Every non-trivial task starts with a read-only pass. The agent must state what it verified and what it did not.
2. **Phases with approval gates.** Routine, reversible work runs autonomously; anything irreversible, costly or publicly visible (push, deploy, production data, secrets, DNS, making something public) stops for a human decision.
3. **Deterministic first, LLM as second opinion, human approves.** Rule engines give reproducible coverage; the model adds judgement and must cite its sources; a person makes the final call and does the sending.
4. **Never invent data.** An honest empty state beats plausible fabricated content. Missing business rules become documented open questions, not silent assumptions.
5. **Never expose secrets.** Secrets live in the environment's configuration, never in versioned files, logs, commits or deploy packages. Public endpoints use explicit field allowlists.

## How to use

Copy the fenced block of a template, replace the `{{placeholders}}`, and delete optional lines that do not apply. Templates 09–11 are meant to live as files (agent system prompts or repository instruction files); the others are pasted as task prompts. Template 12 is a snippet designed to be appended to any of the others.

## License

[MIT](LICENSE) © 2026 Henry Noguchi
