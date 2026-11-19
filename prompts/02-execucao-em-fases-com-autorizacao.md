# 02 · Execução em fases com pontos de autorização explícitos

## Quando usar

Quando o plano já foi aprovado e você quer que o agente trabalhe com autonomia nas operações rotineiras e reversíveis, mas pare obrigatoriamente antes de ações irreversíveis, caras ou visíveis para terceiros (push, deploy, mudança de infraestrutura, dados reais, visibilidade pública).

## Template

```text
Plano aprovado: {{REFERÊNCIA_AO_PLANO_OU_FASE}}.

Trabalhe de forma autônoma até concluir {{ESCOPO: ex. todos os itens P0/P1}}.
Não me peça aprovação para operações rotineiras e reversíveis DENTRO de
{{DIRETÓRIO_DO_PROJETO}}:
- ler e editar arquivos do projeto;
- git status, diff, add e commit LOCAL;
- instalar dependências declaradas do projeto;
- lint, typecheck, testes, build;
- limpar caches e artefatos gerados;
- iniciar e reiniciar o servidor local.

PARE e me consulte ANTES de:
- push para qualquer remoto;
- deploy ou alteração de infraestrutura ({{PROVEDOR_DE_NUVEM}}, DNS, domínio, certificado, plano/tier);
- qualquer escrita em banco que não seja o local de desenvolvimento;
- migrations destrutivas ou não previstas no plano;
- criação, alteração ou remoção de credenciais e segredos;
- tornar qualquer coisa pública (repositório, página, endpoint, artefato);
- operações fora de {{DIRETÓRIO_DO_PROJETO}};
- mudança arquitetural difícil de reverter;
- qualquer ação que possa gerar cobrança externa.

Fases:
1. {{FASE_1}} → validação: {{CRITÉRIO_1}}
2. {{FASE_2}} → validação: {{CRITÉRIO_2}}
3. {{FASE_N}} → validação: {{CRITÉRIO_N}}

Ao fim de cada fase, registre: o que mudou, arquivos tocados, comandos de
validação executados e resultado. Fora das exceções acima, não espere minha
confirmação entre fases.

Ao concluir, me entregue: resumo, lista de commits locais, testes/lint/
typecheck/build com resultado, pendências e a lista exata das ações que
aguardam minha autorização.
```

## Por que funciona

- A lista positiva (pode fazer) e a negativa (pare antes) removem ambiguidade; o agente não precisa adivinhar o seu apetite a risco.
- O critério que separa as duas listas é reversibilidade e alcance, não esforço: um commit local se desfaz, um deploy não.
- Validação por fase evita acumular erros que só aparecem no fim.
- O relatório final com "ações aguardando autorização" transforma a parada em decisão rápida, não em nova investigação.

## Exemplo anonimizado

> Contexto: estabilização de um portal interno antes do lançamento.
>
> "Continue sozinho em todos os P0/P1 até validar build, testes, lint e typecheck. Pode commitar localmente. Pare antes de push, deploy, alteração destrutiva de dados reais, mudança de segredos ou qualquer coisa fora desta pasta."

## Armadilhas

- Autorizar "pode seguir até o fim" sem a lista de paradas: o agente pode interpretar um deploy como parte natural da conclusão.
- Esquecer "tornar público": publicar um repositório ou uma página é tão irreversível quanto um deploy.
- Uma liberação de recurso (ex.: novo plano de hospedagem disponível) não é autorização para usá-lo. Diga isso explicitamente.
- Não exigir critério de validação por fase: "concluído" vira "compilou".
