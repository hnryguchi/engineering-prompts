# Engineering Prompts

**Português** · [English](README.en.md)

Biblioteca de modelos de prompt reutilizáveis para trabalhar com agentes de código (IA) em software que vai para produção: auditorias, execução em fases com pontos de aprovação, deploys seguros, desenvolvimento guiado por especificação e agentes revisores especialistas de domínio.

> Modelos destilados de prompts que usei na construção de sistemas em produção com agentes de código (Claude Code).

Cada modelo traz quando usar, o texto do prompt com `{{placeholders}}`, por que funciona, um exemplo anonimizado e as armadilhas mais comuns. Todos os exemplos são genéricos; nenhum dado real de cliente, pessoa ou infraestrutura foi incluído.

## Índice

| # | Modelo | Finalidade |
|---|--------|------------|
| 01 | [Auditoria somente leitura](prompts/01-auditoria-somente-leitura.md) | Auditoria somente leitura do estado atual antes de qualquer alteração; termina em um plano aguardando aprovação |
| 02 | [Execução em fases com autorização](prompts/02-execucao-em-fases-com-autorizacao.md) | Execução autônoma de trabalho rotineiro e reversível, com pontos de parada explícitos (push, deploy, dados, segredos, visibilidade) |
| 03 | [Retomar de onde parou](prompts/03-retomar-de-onde-parou.md) | Retomada de uma sessão interrompida sem recomeçar nem desfazer trabalho correto |
| 04 | [Isolamento de projeto](prompts/04-isolamento-de-projeto.md) | Fronteiras rígidas para que o agente nunca leia nem altere outros projetos, contas ou recursos de nuvem |
| 05 | [Checklist pré-deploy e migração](prompts/05-checklist-pre-deploy-e-migracao.md) | Autorização de produção estreita e verificável, incluindo migration de banco de dados |
| 06 | [Especificação completa de produto](prompts/06-especificacao-completa-de-produto.md) | Construção guiada por especificação de um app novo com vários componentes, com regras de privacidade, prioridades e roteiro de aceitação |
| 07 | [Auditoria final de verificação](prompts/07-auditoria-final-de-verificacao.md) | Passada somente de verificação, com tabela PASS/FAIL; sem novas funcionalidades |
| 08 | [Automação de planilha](prompts/08-automacao-de-planilha.md) | Automação de planilhas: primeiro a análise, depois mapeamento por cabeçalho com score de confiança e recusa segura |
| 09 | [Agente revisor especialista](prompts/09-agente-revisor-especialista.md) | System prompt: especialista de domínio que confirma ou descarta achados determinísticos, com fonte e justificativa |
| 10 | [Agente redator que não envia](prompts/10-agente-redator-que-nao-envia.md) | System prompt: agente redator que verifica de novo, segue o estilo de quem revisa e só produz rascunhos |
| 11 | [AGENTS.md do repositório](prompts/11-agents-md-do-repositorio.md) | Regras duradouras do repositório: arquitetura, comandos, deploy, segurança, "nunca sem autorização" |
| 12 | [Dados honestos e estado vazio](prompts/12-dados-honestos-e-estado-vazio.md) | Trecho reutilizável: nunca inventar dados nem regras de negócio; zero vs. ausente vs. indisponível; isolamento de DEMO |
| 13 | [Reproduzir, corrigir, validar](prompts/13-correcao-reproduzir-corrigir-validar.md) | Correção de defeito com reprodução obrigatória, causa raiz, sem mascaramento cosmético e com evidência no navegador |

## Princípios

1. **Ler antes de escrever.** Toda tarefa não trivial começa com uma passada somente leitura. O agente deve declarar o que verificou e o que não verificou.
2. **Fases com pontos de aprovação.** Trabalho rotineiro e reversível roda de forma autônoma; qualquer ação irreversível, custosa ou publicamente visível (push, deploy, dados de produção, segredos, DNS, tornar algo público) para e aguarda decisão humana.
3. **Determinístico primeiro, LLM como segunda opinião, humano aprova.** Motores de regras dão cobertura reprodutível; o modelo acrescenta julgamento e precisa citar suas fontes; uma pessoa toma a decisão final e faz o envio (human-in-the-loop).
4. **Nunca inventar dados.** Um estado vazio honesto é melhor que conteúdo fabricado plausível. Regras de negócio ausentes viram perguntas em aberto documentadas, não suposições silenciosas.
5. **Nunca expor segredos.** Segredos ficam na configuração do ambiente, nunca em arquivos versionados, logs, commits ou pacotes de deploy. Endpoints públicos usam allowlists explícitas de campos.

## Como usar

Copie o bloco de código de um modelo, substitua os `{{placeholders}}` e apague as linhas opcionais que não se aplicam. Os modelos 09 a 11 foram feitos para viver como arquivos (system prompts de agentes ou arquivos de instruções do repositório); os demais são colados como prompts de tarefa. O modelo 12 é um trecho pensado para ser anexado a qualquer um dos outros.

## Licença

[MIT](LICENSE) © 2026 Henry Noguchi
