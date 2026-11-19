# 04 · Isolamento de projeto (não tocar em outros projetos ou contas)

## Quando usar

Quando a mesma máquina, conta ou nuvem hospeda vários projetos (pessoais e profissionais, de clientes diferentes, ou um app novo ao lado de um app em produção) e você precisa garantir que o agente trabalhe só dentro de uma fronteira bem definida.

## Template

```text
ISOLAMENTO TOTAL DO PROJETO

Diretório autorizado, e somente ele: {{CAMINHO_DO_PROJETO}}
Antes de qualquer alteração, confirme o diretório atual.

Regras obrigatórias:
- Não ler, pesquisar, copiar ou importar nada de {{OUTRAS_PASTAS_OU_ORGANIZAÇÕES}}.
- Não reutilizar código, imagens, logos, documentos ou dados de {{OUTROS_PROJETOS}}.
- Não acessar armazenamento sincronizado corporativo ou compartilhado ({{SERVIÇOS}}).
- Não usar contas pessoais em ambiente corporativo, nem o contrário.
- Todo código, conteúdo, configuração, controle de versão e assets devem
  existir exclusivamente dentro de {{CAMINHO_DO_PROJETO}}.

Em infraestrutura:
- Não reutilizar nem alterar recursos de {{APP_EXISTENTE}} (hospedagem, plano,
  banco, configurações, domínio).
- Criar recursos novos só depois de me apresentar opções e custo.
- Publicar somente {{ESTE_PROJETO}}; nunca republicar outro app junto.

Se qualquer tarefa exigir acesso fora dessas fronteiras, PARE e peça
autorização, explicando por que é necessário.

No relatório final, confirme explicitamente que nenhum arquivo ou recurso
fora de {{CAMINHO_DO_PROJETO}} foi lido ou alterado.
```

## Por que funciona

- Agentes procuram contexto de forma agressiva; sem fronteira escrita, "aproveitar um componente do outro projeto" parece boa prática para eles.
- A regra cobre leitura, não só escrita: o vazamento de dados de cliente começa quando o agente lê e reaproveita.
- A confirmação do diretório no início e a declaração no fim criam dois pontos de verificação baratos.
- Estender o isolamento para a nuvem evita o caso clássico de um deploy novo sobrescrever o app existente no mesmo plano.

## Exemplo anonimizado

> Contexto: desenvolvedor criando um portfólio pessoal no mesmo computador que usa para o trabalho.
>
> "Este é um projeto pessoal. O único diretório autorizado é a pasta do portfólio. Não leia nem reutilize nada das pastas da empresa, nem do armazenamento corporativo. Se precisar de algo externo, pare e me pergunte."

## Armadilhas

- Proibir só a escrita: a leitura de pastas vizinhas já contamina o resultado.
- Esquecer ferramentas que puxam arquivos implicitamente (build que empacota pastas locais, buscas globais, extensões de navegador logadas em contas pessoais).
- Deixar o agente "decidir" se é razoável hospedar o app novo no plano do app antigo: peça as opções e decida você.
- Não pedir a confirmação final de isolamento; sem ela, você não tem evidência para mostrar a terceiros.
