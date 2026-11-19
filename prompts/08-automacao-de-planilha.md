# 08 · Automação de planilha e rotinas de escritório

## Quando usar

Quando uma equipe preenche formulários em planilhas (Excel/Sheets) e você quer automatizar registro, exportação ou geração de documentos sem quebrar o arquivo que as pessoas já usam. Funciona em duas etapas: análise da estrutura existente e, depois de aprovada, implementação com testes em cópias temporárias.

## Template

```text
ETAPA 1 — ANÁLISE (não altere nada)

Arquivo: {{NOME_DO_ARQUIVO}} (abas: {{ABAS}}).
Inspecione o arquivo por completo: abas, tabelas nomeadas, intervalos,
validações, fórmulas, macros existentes, campos fixos vs. variáveis.
Pode usar {{FERRAMENTA_DE_INSPEÇÃO: ex. Python/openpyxl}} apenas para leitura.

Objetivo desejado:
- o usuário preenche {{ABA_DE_FORMULÁRIO}} e clica em "{{AÇÃO}}";
- os dados viram uma NOVA linha em {{ABA_DE_BASE}}, sem sobrescrever registros;
- limpar só os campos variáveis, mantendo os fixos ({{CAMPOS_FIXOS}});
- {{FUNCIONALIDADES_FUTURAS: ex. exportar para outra planilha, gerar PDF}}.

Entregue: descrição da estrutura encontrada, opções de implementação
(macro, script externo, fórmulas) com prós e contras, e sua recomendação.
Não implemente até eu aprovar.

ETAPA 2 — IMPLEMENTAÇÃO (após aprovação)

Regras de dados:
- mapear colunas sempre pelo NOME do cabeçalho, nunca por posição fixa;
- tabela central de aliases editável para nomes equivalentes ({{EXEMPLOS}});
- correspondência por pontuação de confiança: aceitar destino com pelo menos
  {{N}} correspondências fortes; palavras soltas valem menos que nomes completos;
- empate próximo entre candidatos: NÃO gravar; perguntar ao usuário;
- destino incompatível: recusar sem escrever nada;
- adicionar linha nova, preservar tipos, nunca sobrescrever;
- ao final, informar quantos campos foram exportados e quais não existiam no destino.

Restrições de interface: {{ex. manter só o botão X, visual atual, não alterar o formulário}}.

Testes obrigatórios com arquivos temporários gerados por você:
1. destino com apenas parte das colunas → deve aceitar;
2. destino com nomes diferentes cobertos por aliases → deve aceitar;
3. destino incompatível → deve recusar sem escrever;
4. duas abas compatíveis → deve detectar ambiguidade e perguntar.

Entrega: resumo da lógica, aliases existentes, como funciona a pontuação,
testes executados com resultado.
```

## Por que funciona

- A etapa de análise obrigatória evita a reconstrução do arquivo do zero, que destrói formatação e hábitos da equipe.
- Mapear por nome com aliases e pontuação torna a automação robusta a planilhas de destino que mudam ou têm só parte das colunas.
- A regra "na dúvida, não grava" protege a base de dados de escritas silenciosamente erradas, o defeito mais caro em planilhas.
- Os quatro cenários de teste cobrem caminho feliz, variação, rejeição e ambiguidade.

## Exemplo anonimizado

> Contexto: equipe de campo registra abordagens a embarcações em uma ficha de planilha e precisa exportar os registros para planilhas de órgãos diferentes, cada uma com seu conjunto de colunas.
>
> O critério inicial de "50% dos campos devem existir" rejeitava destinos válidos com 8 colunas; a troca por "3 ou mais correspondências fortes + desempate seguro" resolveu sem perder a segurança.

## Armadilhas

- Critério de compatibilidade por percentual fixo: destinos legítimos com poucas colunas são rejeitados.
- Posições fixas de coluna: a primeira coluna inserida por alguém quebra tudo.
- Testar no arquivo real da equipe em vez de cópias temporárias.
- Não restaurar a formatação original quando ela já foi desconfigurada; peça o backup em vez de inventar um padrão novo.
