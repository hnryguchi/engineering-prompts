# 06 · Desenvolvimento guiado por especificação: produto novo do zero

## Quando usar

Quando você vai pedir a um agente que construa um aplicativo novo, funcional e testável, e não um mockup. Vale para sistemas com vários componentes (agente local, API, banco, painel web) em que requisitos não funcionais, como privacidade, segurança e resiliência, pesam tanto quanto as telas.

## Template

```text
# ESPECIFICAÇÃO — {{NOME_DO_PRODUTO}}

## Missão
Crie um projeto NOVO em {{CAMINHO_DO_NOVO_PROJETO}}. Não altere {{PROJETOS_EXISTENTES}}.
Antes de implementar: examine o workspace, identifique a stack em uso e os
padrões visuais existentes (use outros projetos só como referência, sem modificá-los).
O resultado deve ser uma aplicação funcional e testável localmente, não um mockup.

## 1. Contexto
{{QUEM_USA, EM_QUE_SITUAÇÃO, QUAIS_SISTEMAS_JÁ_EXISTEM}}
Restrição crítica: {{ex. o produto é camada auxiliar e não pode entrar no caminho
crítico do sistema principal; se falhar, nada do que já funciona pode parar}}.

## 2. Objetivo operacional
O usuário precisa conseguir verificar objetivamente:
- {{CAPACIDADE_1}}
- {{CAPACIDADE_N}}
Regra de interpretação: o sistema apresenta fatos; não tira conclusões que os
dados não sustentam ({{ex. "sem interação" ≠ "sem trabalho"}}).

## 3. Privacidade, ética e transparência
- O que é coletado: {{LISTA}}
- O que NUNCA é coletado ou implementado: {{LISTA_DE_PROIBIÇÕES}}
- Como o usuário monitorado/afetado é informado: {{MECANISMO_VISÍVEL}}

## 4. Arquitetura e stack
Componentes mínimos: {{COMPONENTES}}. Justifique a stack escolhida.

## 5. Requisitos funcionais (uma seção por capacidade)
Para cada uma: comportamento, estados possíveis, limites configuráveis,
casos de borda, o que acontece offline/com falha.

## 6. Dados
Modelo (entidades, relações, índices), migrations, retenção, auditoria.
Seeds apenas para desenvolvimento.
Modo DEMO claramente rotulado ("DEMO" vs. "FONTE: REAL"); nunca misturar em silêncio.

## 7. Segurança e perfis
Autenticação, papéis, o que cada papel vê e faz, trilha de auditoria.

## 8. UX
Estados importantes (carregando, vazio, erro, offline, sem permissão),
responsividade, acessibilidade, movimento reduzido.

## 9. Desempenho e resiliência
Orçamento de CPU/RAM/rede, comportamento com internet instável, reenvio.

## 10. Prioridade (em caso de conflito)
1. funcionalidade real; 2. segurança; 3. estabilidade; 4. consumo;
5. tempo real; 6. UX; 7. identidade visual; 8. animações.

## 11. Autonomia e serviços externos
Pode, sem pedir: criar arquivos na nova pasta, instalar dependências, rodar
migrations e banco LOCAIS, testes, builds.
Se faltar credencial de serviço externo ({{LISTA}}): não invente; prepare a
integração e siga com o que é possível localmente.

## 12. Cenário de aceitação (roteiro ponta a ponta)
{{PASSO_A_PASSO_OBSERVÁVEL: ação do usuário → o que deve aparecer no painel}}

## 13. Auditoria final (checklist)
[ ] {{ITEM_VERIFICÁVEL_1}}
[ ] Builds de todos os componentes passam
[ ] Testes passam
[ ] Proibições de privacidade verificadas (ex.: conteúdo sensível NÃO armazenado)
[ ] README, arquitetura e privacidade documentados

## 14. Relatório final
O que está 100% funcional, parcial, dependente de infraestrutura externa;
como iniciar, testar cada cenário, onde ficam os dados, como apagar dados de
teste, consumo medido, riscos, próximos passos para produção.

Ciclo esperado: analisar → arquitetar → implementar → integrar → executar →
testar → corrigir → testar de novo → refinar → documentar → auditar.
```

## Por que funciona

- A missão e as restrições críticas vêm antes das funcionalidades, então moldam todas as decisões seguintes.
- A lista explícita do que não implementar (ex.: captura de conteúdo digitado, persistência oculta) impede que o agente "complete" o produto de forma antiética.
- A ordem de prioridade resolve antecipadamente os conflitos que o agente encontraria sozinho (visual bonito vs. detecção que funciona).
- O cenário de aceitação ponta a ponta e o checklist final transformam "pronto" em algo observável.

## Exemplo anonimizado

> Contexto: equipe que passa a trabalhar remotamente e precisa de telemetria operacional transparente (online, ativo/ocioso, processos críticos em execução), sem nunca registrar o que é digitado e sem interferir nos softwares operacionais.
>
> O roteiro de aceitação descreve: operador inicia o turno → painel mostra ATIVO → operador para de interagir → após o limite, OCIOSO → um processo crítico é fechado → alerta aparece sem recarregar a página → processo reaberto → alerta resolvido.

## Armadilhas

- Especificar só telas e esquecer o que acontece quando algo falha.
- Não declarar o que é proibido coletar: agentes otimizam para "mais dados".
- Pedir "identidade visual caprichada" sem ordem de prioridade: o agente gasta o tempo em animações.
- Usar nomes reais de pessoas no roteiro de aceitação de uma especificação que será versionada ou compartilhada; prefira papéis ("operador A").
- Specs muito longas sem checklist final: nada garante que cada seção foi lida.
