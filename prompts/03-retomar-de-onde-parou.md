# 03 · Retomar exatamente de onde parou

## Quando usar

Depois de uma interrupção (limite de contexto, queda de sessão, troca de máquina, nova conversa) em um trabalho longo que já tem alterações corretas na árvore de trabalho. O risco principal é o agente recomeçar do zero, desfazer o que estava certo ou reabrir decisões já tomadas.

## Template

```text
Continue exatamente de onde parou. Não reinicie a tarefa e não reconstrua
nada do zero.

Último ponto conhecido: {{ÚLTIMA_ATIVIDADE_EM_ANDAMENTO}}.

Antes de alterar qualquer coisa:
1. Verifique o que já foi feito (git status/diff, notas de progresso,
   {{ARQUIVO_DE_ESTADO_OU_HANDOFF}}).
2. Liste em poucas linhas o que considera concluído e o que está pendente.
3. Não desfaça alterações que já estão corretas.

Próximo passo imediato:
{{PASSO_IMEDIATO: ex. confirmar se o defeito X é real ou artefato da captura}}
- Se for real: corrija, teste em {{CENÁRIOS}} e confirme que não regrediu {{ÁREAS_VIZINHAS}}.
- Não esconda o problema removendo o componente afetado.

Depois, continue a lista que já estava em andamento:
- {{ITEM_1}}
- {{ITEM_2}}
- {{ITEM_N}}

Regras que continuam valendo:
- Novas regras de negócio só a partir de {{FONTE_AUTORIZADA: ex. comentários do responsável, documento oficial}}.
  Quando algo não estiver definido nessa fonte, não suponha: registre como pendência.
- Escopo restrito a {{DIRETÓRIO_OU_MÓDULO}}.
- {{RESTRIÇÕES_DE_AUTORIZAÇÃO: ex. sem push/deploy}}

Ao concluir esta etapa, informe: o que foi alterado nesta retomada, o que já
estava pronto e foi preservado, e as pendências reais que sobraram.
```

## Por que funciona

- Obrigar a reconstrução do estado a partir do git e das notas, antes de agir, substitui a memória perdida por evidência.
- Nomear o último ponto conhecido impede que o agente escolha por conta própria onde recomeçar.
- Repetir as regras que continuam valendo evita que restrições de sessões anteriores se percam junto com o contexto.
- Separar "alterado agora" de "preservado" deixa auditável o efeito da retomada.

## Exemplo anonimizado

> Contexto: site pessoal em finalização, sessão interrompida no meio de um ajuste responsivo.
>
> "Continue de onde parou. O último ponto foi a investigação de um possível bug mobile na seção de linha do tempo. Primeiro confirme se é bug real ou artefato da captura. Se for real, corrija e teste em 390x844 e 375 px, sem remover a linha do tempo. Depois siga com i18n, lint, typecheck, build e QA visual."

## Armadilhas

- Não informar o último ponto: o agente tende a reabrir a etapa que lhe parece mais "lógica", não a que estava em curso.
- Mandar "continue" sem repetir restrições de escopo e autorização.
- Não manter um arquivo de estado (handoff) no repositório: toda retomada vira arqueologia.
- Aceitar que "pendências" incluam itens que na verdade só não foram tentados; peça a distinção.
